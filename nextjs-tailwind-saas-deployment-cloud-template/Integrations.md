"use client";

import { useRef } from "react";
import { motion, useInView, Variants } from "motion/react";
import { Zap } from "lucide-react";
import { integrationsConfig } from "@/lib/config";
import { cn } from "@/lib/utils";

// ─── Animation variants ───────────────────────────────────────
const fadeUp: Variants = {
  hidden: { opacity: 0, y: 24 },
  visible: (delay = 0) => ({
    opacity: 1,
    y: 0,
    transition: { duration: 0.55, ease: [0.22, 1, 0.36, 1], delay },
  }),
};

// ─── Category color map ───────────────────────────────────────
const categoryStyle: Record<string, { bg: string; text: string; border: string }> = {
  Git: {
    bg: "oklch(0.72 0.16 195 / 8%)",
    text: "oklch(0.72 0.16 195)",
    border: "oklch(0.72 0.16 195 / 20%)",
  },
  Database: {
    bg: "oklch(0.72 0.17 155 / 8%)",
    text: "oklch(0.72 0.17 155)",
    border: "oklch(0.72 0.17 155 / 20%)",
  },
  Runtime: {
    bg: "oklch(0.75 0.16 85 / 8%)",
    text: "oklch(0.75 0.16 85)",
    border: "oklch(0.75 0.16 85 / 20%)",
  },
  Framework: {
    bg: "oklch(0.60 0.14 280 / 8%)",
    text: "oklch(0.60 0.14 280)",
    border: "oklch(0.60 0.14 280 / 20%)",
  },
  "DNS / CDN": {
    bg: "oklch(0.65 0.22 25 / 8%)",
    text: "oklch(0.65 0.22 25)",
    border: "oklch(0.65 0.22 25 / 20%)",
  },
};

// ─── Integration card ─────────────────────────────────────────
function IntegrationCard({
  item,
  index,
}: {
  item: (typeof integrationsConfig.items)[number];
  index: number;
}) {
  const Icon = item.icon;
  const style = categoryStyle[item.category] ?? categoryStyle.Git;

  return (
    <motion.div
      custom={0.1 + index * 0.07}
      variants={fadeUp}
      className={cn(
        "group relative flex flex-col items-center justify-center gap-3.5",
        "rounded-xl border border-border bg-card px-5 py-7",
        "overflow-hidden transition-all duration-300 cursor-default",
        "hover:border-[--card-border-hover] hover:shadow-[0_0_32px_var(--card-glow)]"
      )}
      style={{
        "--card-border-hover": style.border,
        "--card-glow": style.bg,
      } as React.CSSProperties}
    >
      {/* Top edge highlight */}
      <div
        className="pointer-events-none absolute inset-x-0 top-0 h-px opacity-0 transition-opacity duration-300 group-hover:opacity-100"
        style={{
          background: `linear-gradient(90deg, transparent, ${style.text}, transparent)`,
        }}
        aria-hidden="true"
      />

      {/* Corner glow */}
      <div
        className="pointer-events-none absolute -top-6 -right-6 size-24 rounded-full opacity-0 transition-opacity duration-500 group-hover:opacity-100 blur-2xl"
        style={{ background: style.bg }}
        aria-hidden="true"
      />

      {/* Icon container */}
      <div
        className="relative flex items-center justify-center size-12 rounded-xl border transition-all duration-300 group-hover:scale-105"
        style={{
          background: style.bg,
          borderColor: style.border,
        }}
      >
        <Icon
          className="size-6"
          style={{ color: style.text }}
          strokeWidth={1.5}
          aria-hidden="true"
        />
      </div>

      {/* Name */}
      <div className="flex flex-col items-center gap-2">
        <span className="text-sm font-semibold text-foreground">{item.name}</span>

        {/* Category badge */}
        <span
          className="inline-flex items-center px-2 py-0.5 rounded-full text-[10px] font-medium border"
          style={{
            background: style.bg,
            color: style.text,
            borderColor: style.border,
          }}
        >
          {item.category}
        </span>
      </div>

      {/* Bottom gradient strip */}
      <div
        className="pointer-events-none absolute inset-x-0 bottom-0 h-[2px] opacity-0 transition-opacity duration-300 group-hover:opacity-50"
        style={{
          background: `linear-gradient(90deg, transparent 10%, ${style.text} 50%, transparent 90%)`,
        }}
        aria-hidden="true"
      />
    </motion.div>
  );
}

// ─── Center hub decoration ────────────────────────────────────
// A CloudPulse logo node in the center with radiating connection lines
function HubDecoration({ inView }: { inView: boolean }) {
  // 8 spokes for 8 integration cards
  const spokes = Array.from({ length: 8 }, (_, i) => {
    const angle = (i / 8) * 360;
    return angle;
  });

  return (
    <div className="relative flex items-center justify-center py-10 pointer-events-none" aria-hidden="true">
      <div className="relative flex items-center justify-center size-48">
        {/* Outer ring */}
        <motion.div
          initial={{ opacity: 0, scale: 0.7 }}
          animate={inView ? { opacity: 1, scale: 1 } : {}}
          transition={{ duration: 0.7, ease: [0.22, 1, 0.36, 1], delay: 0.2 }}
          className="absolute size-48 rounded-full border border-primary/10"
        />

        {/* Middle ring */}
        <motion.div
          initial={{ opacity: 0, scale: 0.7 }}
          animate={inView ? { opacity: 1, scale: 1 } : {}}
          transition={{ duration: 0.6, ease: [0.22, 1, 0.36, 1], delay: 0.3 }}
          className="absolute size-32 rounded-full border border-primary/15"
        />

        {/* Spokes */}
        {spokes.map((angle, i) => (
          <motion.div
            key={i}
            initial={{ opacity: 0, scaleX: 0 }}
            animate={inView ? { opacity: 1, scaleX: 1 } : {}}
            transition={{ duration: 0.5, delay: 0.35 + i * 0.04, ease: "easeOut" }}
            className="absolute w-24 h-px origin-left"
            style={{
              background:
                "linear-gradient(90deg, oklch(0.72 0.16 195 / 25%), transparent)",
              transform: `rotate(${angle}deg)`,
              left: "50%",
              top: "calc(50% - 0.5px)",
            }}
          />
        ))}

        {/* Spoke end dots */}
        {spokes.map((angle, i) => {
          const rad = (angle * Math.PI) / 180;
          const r = 88;
          const x = Math.cos(rad) * r;
          const y = Math.sin(rad) * r;
          return (
            <motion.div
              key={`dot-${i}`}
              initial={{ opacity: 0, scale: 0 }}
              animate={inView ? { opacity: 1, scale: 1 } : {}}
              transition={{ duration: 0.3, delay: 0.55 + i * 0.04, ease: "backOut" }}
              className="absolute size-1.5 rounded-full bg-primary/40"
              style={{
                left: `calc(50% + ${x}px - 3px)`,
                top: `calc(50% + ${y}px - 3px)`,
              }}
            />
          );
        })}

        {/* Center hub */}
        <motion.div
          initial={{ opacity: 0, scale: 0.5 }}
          animate={inView ? { opacity: 1, scale: 1 } : {}}
          transition={{ duration: 0.5, ease: "backOut", delay: 0.25 }}
          className="relative z-10 flex items-center justify-center size-14 rounded-2xl bg-primary/10 border border-primary/30 shadow-glow-sm"
        >
          <Zap className="size-6 text-primary" strokeWidth={2} />

          {/* Ping ring */}
          <span className="absolute inline-flex size-full rounded-2xl border border-primary/30 animate-ping opacity-20" />
        </motion.div>
      </div>
    </div>
  );
}

// ─── Background ───────────────────────────────────────────────
function SectionBackground() {
  return (
    <div className="pointer-events-none absolute inset-0 overflow-hidden" aria-hidden="true">
      <div
        className="absolute inset-x-0 top-0 h-px"
        style={{
          background:
            "linear-gradient(90deg, transparent 5%, oklch(0.72 0.16 195 / 12%) 30%, oklch(0.72 0.16 195 / 12%) 70%, transparent 95%)",
        }}
      />
      <div
        className="absolute left-1/2 top-1/2 -translate-x-1/2 -translate-y-1/2 size-[800px] rounded-full opacity-[0.025]"
        style={{
          background:
            "radial-gradient(circle, oklch(0.72 0.16 195) 0%, transparent 65%)",
          filter: "blur(80px)",
        }}
      />
    </div>
  );
}

// ─── Integrations ────────────────────────────────────────────
export function Integrations() {
  const ref = useRef(null);
  const inView = useInView(ref, { once: true, margin: "-80px" });

  const items = integrationsConfig.items;

  // Split 8 items: 4 left, 4 right of center hub
  const left = items.slice(0, 4);
  const right = items.slice(4);

  return (
    <section
      ref={ref}
      aria-labelledby="integrations-heading"
      className="relative isolate px-4 py-20 sm:py-28 lg:py-36"
    >
      <SectionBackground />

      <div className="relative mx-auto max-w-6xl">
        {/* Header */}
        <div className="flex flex-col items-center text-center gap-4 mb-16">
          <motion.p
            custom={0}
            variants={fadeUp}
            initial="hidden"
            animate={inView ? "visible" : "hidden"}
            className="text-xs font-semibold uppercase tracking-widest text-primary"
          >
            {integrationsConfig.label}
          </motion.p>

          <motion.h2
            id="integrations-heading"
            custom={0.08}
            variants={fadeUp}
            initial="hidden"
            animate={inView ? "visible" : "hidden"}
            className="text-3xl sm:text-4xl lg:text-5xl font-extrabold tracking-tighter leading-tight max-w-2xl"
          >
            {integrationsConfig.headline}
          </motion.h2>

          <motion.p
            custom={0.16}
            variants={fadeUp}
            initial="hidden"
            animate={inView ? "visible" : "hidden"}
            className="max-w-xl text-base text-muted-foreground leading-relaxed"
          >
            {integrationsConfig.subheadline}
          </motion.p>
        </div>

        {/* Desktop: hub layout — left grid | hub | right grid */}
        <div className="hidden lg:flex items-center gap-6">
          {/* Left column — 2x2 grid */}
          <motion.div
            initial="hidden"
            animate={inView ? "visible" : "hidden"}
            className="flex-1 grid grid-cols-2 gap-3"
          >
            {left.map((item, i) => (
              <IntegrationCard key={item.name} item={item} index={i} />
            ))}
          </motion.div>

          {/* Center hub */}
          <div className="shrink-0">
            <HubDecoration inView={inView} />
          </div>

          {/* Right column — 2x2 grid */}
          <motion.div
            initial="hidden"
            animate={inView ? "visible" : "hidden"}
            className="flex-1 grid grid-cols-2 gap-3"
          >
            {right.map((item, i) => (
              <IntegrationCard key={item.name} item={item} index={4 + i} />
            ))}
          </motion.div>
        </div>

        {/* Mobile / tablet: uniform grid */}
        <motion.div
          initial="hidden"
          animate={inView ? "visible" : "hidden"}
          className="lg:hidden grid grid-cols-2 sm:grid-cols-4 gap-3"
        >
          {items.map((item, i) => (
            <IntegrationCard key={item.name} item={item} index={i} />
          ))}
        </motion.div>

        {/* "More coming soon" hint */}
        <motion.p
          custom={0.7}
          variants={fadeUp}
          initial="hidden"
          animate={inView ? "visible" : "hidden"}
          className="mt-10 text-center text-xs text-muted-foreground"
        >
          More integrations coming soon —{" "}
          <a
            href="/docs/integrations"
            className="text-primary underline underline-offset-2 hover:opacity-80 transition-opacity"
          >
            request one
          </a>
        </motion.p>
      </div>
    </section>
  );
}
