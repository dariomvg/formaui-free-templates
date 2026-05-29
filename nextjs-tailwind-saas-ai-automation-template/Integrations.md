"use client";

import { useRef, useState } from "react";
import Link from "next/link";
import { motion, useInView } from "motion/react";
import { ArrowRight, Zap } from "lucide-react";
import { Badge } from "@/components/ui/badge";
import { Button } from "@/components/ui/button";
import {
  INTEGRATIONS,
  INTEGRATION_CATEGORIES,
  INTEGRATIONS_STATS,
  type IntegrationCategory,
  type IntegrationStatus,
} from "@/lib/config";
import { cn } from "@/lib/utils";

// ─── Marquee strip ────────────────────────────────────────────────────────────

function MarqueeStrip() {
  // duplicate for seamless loop
  const items = [...INTEGRATIONS, ...INTEGRATIONS];

  return (
    <div className="relative w-full overflow-hidden py-6">
      {/* left fade */}
      <div className="absolute left-0 top-0 bottom-0 w-24 z-10 bg-linear-to-r from-background to-transparent pointer-events-none" />
      {/* right fade */}
      <div className="absolute right-0 top-0 bottom-0 w-24 z-10 bg-linear-to-l from-background to-transparent pointer-events-none" />

      <div
        className="flex gap-3 w-max"
        style={{ animation: "marquee 28s linear infinite" }}
      >
        {items.map((integration, i) => {
          const Icon = integration.icon;
          return (
            <div
              key={`${integration.id}-${i}`}
              className="flex items-center gap-2.5 px-4 py-2.5 rounded-xl border border-border bg-card/50 shrink-0 hover:border-accent/25 hover:bg-card/80 transition-all duration-200 cursor-default"
            >
              <span
                className="flex items-center justify-center size-6 rounded-md shrink-0"
                style={{ backgroundColor: `${integration.color}18` }}
              >
                <Icon
                  className="size-3.5"
                  style={{ color: integration.color }}
                  strokeWidth={1.75}
                />
              </span>
              <span className="text-xs font-medium text-muted-foreground whitespace-nowrap">
                {integration.name}
              </span>
            </div>
          );
        })}
      </div>

      <style>{`
        @keyframes marquee {
          from { transform: translateX(0); }
          to   { transform: translateX(-50%); }
        }
      `}</style>
    </div>
  );
}

// ─── Status pill ──────────────────────────────────────────────────────────────

const STATUS_CONFIG: Record<
  IntegrationStatus,
  { label: string; className: string }
> = {
  connected: {
    label: "Connected",
    className: "border-accent/25 bg-accent/8 text-accent",
  },
  available: {
    label: "Available",
    className: "border-border bg-muted/50 text-muted-foreground",
  },
  coming_soon: {
    label: "Soon",
    className: "border-border/50 bg-muted/30 text-muted-foreground/60",
  },
};

// ─── Integration card ─────────────────────────────────────────────────────────

function IntegrationCard({
  integration,
  index,
  inView,
}: {
  integration: (typeof INTEGRATIONS)[number];
  index: number;
  inView: boolean;
}) {
  const Icon = integration.icon;
  const status = STATUS_CONFIG[integration.status];
  const delay = 0.05 + (index % 4) * 0.06 + Math.floor(index / 4) * 0.08;

  return (
    <motion.div
      initial={{ opacity: 0, scale: 0.95 }}
      animate={inView ? { opacity: 1, scale: 1 } : { opacity: 0, scale: 0.95 }}
      transition={{ delay, duration: 0.4, ease: [0.16, 1, 0.3, 1] }}
      className={cn(
        "group relative flex flex-col gap-3.5 p-4 rounded-xl",
        "border border-border bg-card/50",
        "hover:border-accent/25 hover:bg-card/80",
        "transition-all duration-250 overflow-hidden",
        integration.status === "coming_soon" && "opacity-50 hover:opacity-70"
      )}
    >
      {/* hover top glow */}
      <div
        className="absolute inset-0 opacity-0 group-hover:opacity-100 transition-opacity duration-400 pointer-events-none rounded-xl"
        style={{
          background: `radial-gradient(ellipse 80% 60% at 50% 0%, ${integration.color}0a, transparent)`,
        }}
      />

      {/* icon + status row */}
      <div className="relative flex items-start justify-between">
        <div
          className="flex items-center justify-center size-9 rounded-lg shrink-0 transition-all duration-300 group-hover:scale-105"
          style={{ backgroundColor: `${integration.color}15` }}
        >
          <Icon
            className="size-4.5"
            style={{ color: integration.color }}
            strokeWidth={1.75}
          />
        </div>

        <Badge
          variant="secondary"
          className={cn("text-[10px] h-5 px-2 font-medium border shrink-0", status.className)}
        >
          {integration.popular && integration.status === "available" && (
            <span className="mr-1 size-1 rounded-full bg-current inline-block" />
          )}
          {status.label}
        </Badge>
      </div>

      {/* text */}
      <div className="relative flex flex-col gap-1">
        <div className="flex items-center gap-1.5">
          <h3 className="text-sm font-semibold text-foreground">{integration.name}</h3>
        </div>
        <p className="text-[11px] text-muted-foreground leading-relaxed line-clamp-2">
          {integration.description}
        </p>
      </div>

      {/* category tag */}
      <div className="relative mt-auto">
        <span className="text-[10px] text-muted-foreground/60 uppercase tracking-wider font-medium">
          {integration.category}
        </span>
      </div>
    </motion.div>
  );
}

// ─── Category filter tabs ─────────────────────────────────────────────────────

function CategoryTabs({
  active,
  onChange,
  inView,
}: {
  active: IntegrationCategory | "All";
  onChange: (c: IntegrationCategory | "All") => void;
  inView: boolean;
}) {
  const all = ["All", ...INTEGRATION_CATEGORIES] as const;

  return (
    <motion.div
      initial={{ opacity: 0, y: 12 }}
      animate={inView ? { opacity: 1, y: 0 } : { opacity: 0, y: 12 }}
      transition={{ delay: 0.2, duration: 0.4 }}
      className="flex flex-wrap items-center gap-1.5 justify-center"
    >
      {all.map((cat) => (
        <button
          key={cat}
          onClick={() => onChange(cat as IntegrationCategory | "All")}
          className={cn(
            "px-3 py-1.5 rounded-lg text-xs font-medium transition-all duration-200",
            active === cat
              ? "bg-accent/15 text-accent border border-accent/30"
              : "bg-muted/40 text-muted-foreground border border-transparent hover:bg-muted/70 hover:text-foreground"
          )}
        >
          {cat}
        </button>
      ))}
    </motion.div>
  );
}

// ─── Section ──────────────────────────────────────────────────────────────────

export function Integrations() {
  const ref = useRef<HTMLElement>(null);
  const inView = useInView(ref, { once: true, margin: "-60px" });
  const [activeCategory, setActiveCategory] = useState<IntegrationCategory | "All">("All");

  const filtered =
    activeCategory === "All"
      ? INTEGRATIONS
      : INTEGRATIONS.filter((i) => i.category === activeCategory);

  return (
    <section ref={ref} className="relative py-24 px-4 overflow-hidden">

      {/* ambient glow */}
      <div
        className="absolute top-0 left-1/2 -translate-x-1/2 w-175 h-64 pointer-events-none"
        style={{
          background:
            "radial-gradient(ellipse 80% 100% at 50% 0%, oklch(0.82 0.22 135 / 5%), transparent)",
        }}
      />

      <div className="relative max-w-6xl mx-auto">

        {/* header */}
        <motion.div
          className="flex flex-col items-center text-center mb-10"
          initial={{ opacity: 0, y: 20 }}
          animate={inView ? { opacity: 1, y: 0 } : { opacity: 0, y: 20 }}
          transition={{ duration: 0.5, ease: [0.16, 1, 0.3, 1] }}
        >
          <span className="text-xs font-medium text-accent uppercase tracking-[0.2em] mb-3">
            Integrations
          </span>
          <h2 className="text-3xl sm:text-4xl md:text-5xl font-bold tracking-tight text-foreground mb-4">
            Connect every tool{" "}
            <br className="hidden sm:block" />
            <span className="text-muted-foreground font-normal">your team already uses.</span>
          </h2>
          <p className="text-sm sm:text-base text-muted-foreground max-w-md leading-relaxed mb-6">
            FlowMind works with the apps you depend on. One-click OAuth connections,
            no credentials to manage.
          </p>

          {/* stats row */}
          <div className="flex items-center gap-6">
            <div className="flex items-center gap-2">
              <Zap className="size-3.5 text-accent" />
              <span className="text-sm font-semibold text-foreground">
                {INTEGRATIONS_STATS.total}
              </span>
              <span className="text-sm text-muted-foreground">
                {INTEGRATIONS_STATS.label}
              </span>
            </div>
            <div className="w-px h-4 bg-border" />
            <span className="text-sm text-muted-foreground">
              <span className="text-foreground font-semibold">
                {INTEGRATIONS_STATS.newPerMonth}
              </span>{" "}
              {INTEGRATIONS_STATS.newLabel}
            </span>
          </div>
        </motion.div>

        {/* marquee */}
        <motion.div
          initial={{ opacity: 0 }}
          animate={inView ? { opacity: 1 } : { opacity: 0 }}
          transition={{ delay: 0.15, duration: 0.5 }}
        >
          <MarqueeStrip />
        </motion.div>

        {/* divider */}
        <div className="w-full h-px bg-border my-10" />

        {/* category filter */}
        <div className="mb-8">
          <CategoryTabs
            active={activeCategory}
            onChange={setActiveCategory}
            inView={inView}
          />
        </div>

        {/* grid */}
        <motion.div
          layout
          className="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 gap-3"
        >
          {filtered.map((integration, i) => (
            <IntegrationCard
              key={integration.id}
              integration={integration}
              index={i}
              inView={inView}
            />
          ))}
        </motion.div>

        {/* bottom CTA */}
        <motion.div
          initial={{ opacity: 0, y: 16 }}
          animate={inView ? { opacity: 1, y: 0 } : { opacity: 0, y: 16 }}
          transition={{ delay: 0.55, duration: 0.45 }}
          className="mt-10 flex flex-col sm:flex-row items-center justify-center gap-3"
        >
          <Button
            variant="outline"
            className="gap-2 border-border hover:border-accent/30 hover:bg-accent/8 hover:text-accent text-muted-foreground group transition-all duration-200"
            asChild
          >
            <Link href="/integrations">
              Browse all integrations
              <ArrowRight className="size-3.5 group-hover:translate-x-0.5 transition-transform duration-150" />
            </Link>
          </Button>
          <span className="text-xs text-muted-foreground">
            Missing an integration?{" "}
            <a
              href="mailto:hello@flowmind.ai"
              className="text-accent hover:text-accent/80 underline underline-offset-2 transition-colors"
            >
              Request it →
            </a>
          </span>
        </motion.div>
      </div>
    </section>
  );
}
