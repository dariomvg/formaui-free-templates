"use client";

import { useRef } from "react";
import { motion, useInView } from "motion/react";
import { featuresConfig } from "@/lib/config";
import { cn } from "@/lib/utils";
import { LucideIcon } from "lucide-react";

// ─────────────────────────────────────────────
// TYPES
// ─────────────────────────────────────────────

interface FeatureItem {
  icon: LucideIcon;
  title: string;
  description: string;
}

// ─────────────────────────────────────────────
// FEATURE CARD
// ─────────────────────────────────────────────

interface FeatureCardProps {
  item: FeatureItem;
  index: number;
  inView: boolean;
}

function FeatureCard({ item, index, inView }: FeatureCardProps) {
  const Icon = item.icon;

  // first card spans 2 cols on md+, gets a slightly richer treatment
  const isFeatured = index === 0;

  return (
    <motion.div
      initial={{ opacity: 0, y: 24 }}
      animate={inView ? { opacity: 1, y: 0 } : { opacity: 0, y: 24 }}
      transition={{
        duration: 0.5,
        delay: index * 0.07,
        ease: [0.25, 0.1, 0.25, 1],
      }}
      className={cn(
        isFeatured && "md:col-span-2 lg:col-span-2",
      )}
    >
      <div
        className={cn(
          "group relative h-full rounded-xl p-px overflow-hidden",
          "transition-all duration-300",
          // border gradient via pseudo bg
          "bg-border/40 hover:bg-gradient-to-br hover:from-primary/30 hover:via-border/20 hover:to-transparent",
        )}
      >
        {/* inner card */}
        <div
          className={cn(
            "relative h-full rounded-[11px] p-5 flex gap-4 overflow-hidden",
            isFeatured ? "flex-row items-start" : "flex-col",
            "bg-card",
            "group-hover:bg-[oklch(0.19_0.015_60)]",
            "transition-colors duration-300",
          )}
        >
          {/* ambient glow — appears on hover */}
          <div
            aria-hidden="true"
            className={cn(
              "absolute inset-0 opacity-0 group-hover:opacity-100 transition-opacity duration-500 pointer-events-none",
              "bg-[radial-gradient(ellipse_80%_60%_at_0%_0%,oklch(0.72_0.19_52/8%),transparent)]",
            )}
          />

          {/* icon container */}
          <div
            className={cn(
              "shrink-0 flex items-center justify-center rounded-lg",
              "size-10 border border-primary/20 bg-primary/8",
              "group-hover:border-primary/40 group-hover:bg-primary/15",
              "group-hover:shadow-[var(--glow-sm)]",
              "transition-all duration-300",
            )}
          >
            <Icon
              className="size-4.5 text-primary group-hover:text-accent transition-colors duration-300"
              strokeWidth={1.75}
            />
          </div>

          {/* text */}
          <div className={cn("flex flex-col gap-1.5", isFeatured && "pt-0.5")}>
            <h3 className="text-sm font-semibold text-foreground leading-snug">
              {item.title}
            </h3>
            <p className="text-sm text-muted-foreground leading-relaxed">
              {item.description}
            </p>
          </div>

          {/* featured — decorative corner lines */}
          {isFeatured && (
            <div aria-hidden="true" className="absolute bottom-4 right-4 opacity-10 group-hover:opacity-20 transition-opacity duration-300">
              <svg width="64" height="64" viewBox="0 0 64 64" fill="none">
                <path d="M0 64 L64 0" stroke="currentColor" strokeWidth="0.5" className="text-primary" />
                <path d="M16 64 L64 16" stroke="currentColor" strokeWidth="0.5" className="text-primary" />
                <path d="M32 64 L64 32" stroke="currentColor" strokeWidth="0.5" className="text-primary" />
                <path d="M48 64 L64 48" stroke="currentColor" strokeWidth="0.5" className="text-primary" />
              </svg>
            </div>
          )}
        </div>
      </div>
    </motion.div>
  );
}

// ─────────────────────────────────────────────
// SECTION HEADER
// ─────────────────────────────────────────────

interface SectionHeaderProps {
  eyebrow: string;
  headline: string;
  subheadline: string;
  inView: boolean;
}

function SectionHeader({ eyebrow, headline, subheadline, inView }: SectionHeaderProps) {
  return (
    <motion.div
      initial={{ opacity: 0, y: 20 }}
      animate={inView ? { opacity: 1, y: 0 } : { opacity: 0, y: 20 }}
      transition={{ duration: 0.5, ease: [0.25, 0.1, 0.25, 1] }}
      className="flex flex-col items-center text-center gap-4 max-w-2xl mx-auto"
    >
      {/* eyebrow */}
      <span
        className={cn(
          "inline-flex items-center gap-2 text-xs font-semibold uppercase tracking-widest",
          "text-primary/80",
        )}
      >
        <span
          aria-hidden="true"
          className="h-px w-6 bg-gradient-to-r from-transparent to-primary/60"
        />
        {eyebrow}
        <span
          aria-hidden="true"
          className="h-px w-6 bg-gradient-to-l from-transparent to-primary/60"
        />
      </span>

      <h2 className="text-3xl sm:text-4xl lg:text-5xl font-bold tracking-tight leading-[1.1] text-foreground">
        {headline}
      </h2>

      <p className="text-base sm:text-lg text-muted-foreground leading-relaxed">
        {subheadline}
      </p>
    </motion.div>
  );
}

// ─────────────────────────────────────────────
// FEATURES
// ─────────────────────────────────────────────

export function Features() {
  const ref = useRef<HTMLElement>(null);
  const inView = useInView(ref, { once: true, margin: "-100px" });

  return (
    <section
      ref={ref}
      id="features"
      aria-label="Features"
      className="relative py-24 sm:py-32 overflow-hidden"
    >
      {/* section ambient */}
      <div aria-hidden="true" className="absolute inset-0 pointer-events-none">
        <div
          className="absolute inset-x-0 top-0 h-px"
          style={{
            background:
              "linear-gradient(to right, transparent, oklch(0.72 0.19 52 / 20%), transparent)",
          }}
        />
        <div
          className="absolute left-1/2 -translate-x-1/2 top-0 size-[600px] -translate-y-1/2 rounded-full blur-3xl opacity-10"
          style={{ background: "oklch(0.72 0.19 52 / 60%)" }}
        />
      </div>

      <div className="relative z-10 mx-auto max-w-6xl px-4">
        {/* header */}
        <div className="mb-14">
          <SectionHeader
            eyebrow={featuresConfig.eyebrow}
            headline={featuresConfig.headline}
            subheadline={featuresConfig.subheadline}
            inView={inView}
          />
        </div>

        {/* grid — 1 col mobile / 2 col md / 3 col lg */}
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-3">
          {featuresConfig.items.map((item, i) => (
            <FeatureCard key={item.title} item={item} index={i} inView={inView} />
          ))}
        </div>

        {/* bottom divider fade */}
        <div
          aria-hidden="true"
          className="absolute bottom-0 left-0 right-0 h-24 pointer-events-none"
          style={{
            background:
              "linear-gradient(to top, var(--background) 0%, transparent 100%)",
          }}
        />
      </div>
    </section>
  );
}
