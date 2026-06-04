"use client";

import { useRef } from "react";
import { motion, useInView } from "motion/react";
import { howItWorksConfig } from "@/lib/config";
import { cn } from "@/lib/utils";
import { LucideIcon } from "lucide-react";


// ─────────────────────────────────────────────
// HOW IT WORKS
// ─────────────────────────────────────────────

interface StepCardProps {
  number: string;
  icon: LucideIcon;
  title: string;
  description: string;
  index: number;
  inView: boolean;
  isLast: boolean;
}

function StepCard({
  number,
  icon: Icon,
  title,
  description,
  index,
  inView,
  isLast,
}: StepCardProps) {
  return (
    <div className="relative flex flex-col items-center text-center">
      {/* connector line between steps */}
      {!isLast && (
        <div
          aria-hidden="true"
          className="hidden lg:block absolute top-10 left-[calc(50%+2.5rem)] right-[calc(-50%+2.5rem)] h-px z-0"
          style={{
            background:
              "linear-gradient(to right, oklch(0.72 0.19 52 / 30%), oklch(0.72 0.19 52 / 5%))",
          }}
        >
          {/* animated dot travelling the line */}
          <motion.span
            className="absolute top-1/2 -translate-y-1/2 size-1 rounded-full bg-primary/60"
            animate={{ left: ["0%", "100%"] }}
            transition={{
              duration: 2.5,
              delay: index * 0.8,
              repeat: Infinity,
              ease: "linear",
            }}
          />
        </div>
      )}

      <motion.div
        initial={{ opacity: 0, y: 24 }}
        animate={inView ? { opacity: 1, y: 0 } : { opacity: 0, y: 24 }}
        transition={{ duration: 0.5, delay: 0.1 + index * 0.15, ease: [0.25, 0.1, 0.25, 1] }}
        className="flex flex-col items-center gap-5 relative z-10"
      >
        {/* step number + icon stack */}
        <div className="relative">
          {/* outer ring */}
          <div
            className={cn(
              "flex items-center justify-center size-20 rounded-2xl",
              "border border-primary/20 bg-card",
              "shadow-[var(--glow-sm)]",
            )}
          >
            <Icon className="size-7 text-primary" strokeWidth={1.5} />
          </div>

          {/* step number badge */}
          <div
            className={cn(
              "absolute -top-2 -right-2 size-6 rounded-full",
              "flex items-center justify-center",
              "bg-primary text-primary-foreground",
              "text-[10px] font-bold",
              "shadow-[var(--glow-sm)]",
            )}
          >
            {index + 1}
          </div>
        </div>

        {/* large number watermark */}
        <div className="flex flex-col items-center gap-2">
          <span
            aria-hidden="true"
            className="text-[4.5rem] font-black leading-none tracking-tighter tabular-nums select-none"
            style={{ color: "oklch(1 0 0 / 3%)" }}
          >
            {number}
          </span>

          {/* title sits over the watermark */}
          <div className="-mt-10 flex flex-col gap-2">
            <h3 className="text-base font-semibold text-foreground">{title}</h3>
            <p className="text-sm text-muted-foreground leading-relaxed max-w-[18rem]">
              {description}
            </p>
          </div>
        </div>
      </motion.div>
    </div>
  );
}

export function HowItWorks() {
  const ref = useRef<HTMLElement>(null);
  const inView = useInView(ref, { once: true, margin: "-100px" });

  return (
    <section
      ref={ref}
      aria-label="How it works"
      className="relative py-24 sm:py-32 overflow-hidden"
    >
      {/* ambient */}
      <div aria-hidden="true" className="absolute inset-0 pointer-events-none">
        <div
          className="absolute inset-x-0 top-0 h-px"
          style={{
            background:
              "linear-gradient(to right, transparent, oklch(0.72 0.19 52 / 15%), transparent)",
          }}
        />
        <div
          className="absolute left-1/4 top-1/2 -translate-y-1/2 size-[500px] rounded-full blur-3xl opacity-[0.06]"
          style={{ background: "oklch(0.72 0.19 52 / 80%)" }}
        />
      </div>

      <div className="relative z-10 mx-auto max-w-6xl px-4">
        {/* header */}
        <motion.div
          initial={{ opacity: 0, y: 20 }}
          animate={inView ? { opacity: 1, y: 0 } : { opacity: 0, y: 20 }}
          transition={{ duration: 0.5, ease: [0.25, 0.1, 0.25, 1] }}
          className="flex flex-col items-center text-center gap-4 max-w-xl mx-auto mb-16"
        >
          <span className="inline-flex items-center gap-2 text-xs font-semibold uppercase tracking-widest text-primary/80">
            <span aria-hidden="true" className="h-px w-6 bg-gradient-to-r from-transparent to-primary/60" />
            {howItWorksConfig.eyebrow}
            <span aria-hidden="true" className="h-px w-6 bg-gradient-to-l from-transparent to-primary/60" />
          </span>

          <h2 className="text-3xl sm:text-4xl lg:text-5xl font-bold tracking-tight leading-[1.1] text-foreground">
            {howItWorksConfig.headline}
          </h2>
        </motion.div>

        {/* steps */}
        <div className="grid grid-cols-1 lg:grid-cols-3 gap-12 lg:gap-6">
          {howItWorksConfig.steps.map((step, i) => (
            <StepCard
              key={step.number}
              {...step}
              index={i}
              inView={inView}
              isLast={i === howItWorksConfig.steps.length - 1}
            />
          ))}
        </div>
      </div>
    </section>
  );
}
