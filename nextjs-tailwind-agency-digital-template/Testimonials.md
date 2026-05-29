"use client";

import { motion, useAnimationFrame, useMotionValue } from "motion/react";
import { useRef, useState } from "react";

const TESTIMONIALS = [
  {
    quote:
      "Vekto took our vague brief and turned it into a product we're genuinely proud of. The process was smooth, the code is clean, and our conversion rate jumped 34% in the first month.",
    name: "Alex Morgan",
    role: "CEO",
    company: "Orbit",
    initials: "AM",
    hue: 142,
  },
  {
    quote:
      "We'd worked with three agencies before Vekto. None of them shipped on time. These guys delivered two days early with zero cut corners. Remarkable.",
    name: "Sofia Chen",
    role: "Founder",
    company: "Madera",
    initials: "SC",
    hue: 160,
  },
  {
    quote:
      "The attention to motion and micro-interactions blew our team away. It's not just a website — it's an experience. Our sales team uses it as a live demo in every pitch.",
    name: "Daniel Osei",
    role: "Head of Product",
    company: "Pulse",
    initials: "DO",
    hue: 148,
  },
  {
    quote:
      "From brand identity to the final deploy, Vekto handled everything with taste and precision. We get compliments on our site every single week.",
    name: "Lena Park",
    role: "CMO",
    company: "Novu",
    initials: "LP",
    hue: 155,
  },
  {
    quote:
      "Fast, responsive, and they actually push back when an idea isn't right. That honesty saved us from a bad decision early on. Exactly what a good partner should do.",
    name: "James Harlow",
    role: "CTO",
    company: "Stackly",
    initials: "JH",
    hue: 138,
  },
  {
    quote:
      "The SEO results alone paid for the project within 90 days. Organic traffic up 180%. I recommend Vekto to every founder I know.",
    name: "Maria Torres",
    role: "Founder",
    company: "Growlio",
    initials: "MT",
    hue: 165,
  },
];

// Duplicate for seamless loop
const ITEMS = [...TESTIMONIALS, ...TESTIMONIALS];

const ease = [0.22, 1, 0.36, 1] as const;
const CARD_WIDTH = 380;
const GAP = 16;
const SPEED = 0.4; // px per frame

function TestimonialCard({
  item,
}: {
  item: (typeof TESTIMONIALS)[number];
}) {
  return (
    <div
      className="relative shrink-0 flex flex-col justify-between p-7 rounded-xl border border-border bg-card
                 hover:border-primary/25 transition-colors duration-300 group"
      style={{ width: CARD_WIDTH }}
    >
      {/* Hover glow */}
      <span
        className="absolute inset-0 rounded-xl opacity-0 group-hover:opacity-20 transition-opacity duration-500 pointer-events-none bg-gradient-primary"
        
        aria-hidden="true"
      />

      {/* Quote mark */}
      <div className="mb-5">
        <svg
          width="28"
          height="22"
          viewBox="0 0 28 22"
          fill="none"
          aria-hidden="true"
          className="text-primary opacity-60"
        >
          <path
            d="M0 22V13.2C0 9.6 0.8 6.6 2.4 4.2C4.08 1.8 6.52 0.28 9.72 0L10.92 2.04C8.6 2.6 6.88 3.68 5.76 5.28C4.72 6.88 4.2 8.72 4.2 10.8H9.6V22H0ZM17.4 22V13.2C17.4 9.6 18.2 6.6 19.8 4.2C21.48 1.8 23.92 0.28 27.12 0L28 2.04C25.68 2.6 23.96 3.68 22.84 5.28C21.8 6.88 21.28 8.72 21.28 10.8H27V22H17.4Z"
            fill="currentColor"
          />
        </svg>
      </div>

      {/* Quote text */}
      <p className="text-sm text-muted-foreground leading-relaxed flex-1 mb-8">
        {item.quote}
      </p>

      {/* Author */}
      <div className="flex items-center gap-3">
        {/* Avatar */}
        <div
          className="shrink-0 w-9 h-9 rounded-full flex items-center justify-center text-xs font-bold"
          style={{
            background: `oklch(0.22 0.06 ${item.hue})`,
            color: `oklch(0.87 0.22 ${item.hue})`,
            boxShadow: `0 0 0 1px oklch(0.87 0.22 ${item.hue} / 20%)`,
          }}
          aria-hidden="true"
        >
          {item.initials}
        </div>

        <div className="min-w-0">
          <p className="text-sm font-semibold text-foreground leading-none mb-1 truncate">
            {item.name}
          </p>
          <p className="text-xs text-muted-foreground truncate">
            {item.role} · {item.company}
          </p>
        </div>

        {/* Company badge */}
        <div className="ml-auto shrink-0">
          <span className="px-2.5 py-1 rounded-md bg-secondary text-xs font-medium text-muted-foreground">
            {item.company}
          </span>
        </div>
      </div>
    </div>
  );
}

function MarqueeRow({
  items,
  reverse = false,
}: {
  items: typeof ITEMS;
  reverse?: boolean;
}) {
  const x = useMotionValue(0);
  const paused = useRef(false);
  const totalWidth = items.length * (CARD_WIDTH + GAP);

  useAnimationFrame(() => {
    if (paused.current) return;
    const current = x.get();
    const next = current - (reverse ? -SPEED : SPEED);
    // Reset when one full set scrolled
    const half = totalWidth / 2;
    x.set(reverse ? (next > 0 ? next - half : next) : next <= -half ? next + half : next);
  });

  return (
    <div
      className="overflow-hidden"
      onMouseEnter={() => { paused.current = true; }}
      onMouseLeave={() => { paused.current = false; }}
    >
      <motion.div
        className="flex gap-4"
        style={{ x }}
      >
        {items.map((item, i) => (
          <TestimonialCard key={`${item.name}-${i}`} item={item} />
        ))}
      </motion.div>
    </div>
  );
}

export const Testimonials = () => {
  return (
    <section className="relative py-24 md:py-32 overflow-hidden">

      {/* Top divider glow */}
      <div
        className="absolute top-0 left-1/2 -translate-x-1/2 w-100 h-px opacity-20"
        style={{
          background:
            "linear-gradient(90deg, transparent, var(--color-primary), transparent)",
        }}
        aria-hidden="true"
      />

      {/* Edge fade masks */}
      <div
        className="absolute inset-y-0 left-0 w-32 z-10 pointer-events-none"
        style={{
          background:
            "linear-gradient(to right, var(--color-background), transparent)",
        }}
        aria-hidden="true"
      />
      <div
        className="absolute inset-y-0 right-0 w-32 z-10 pointer-events-none"
        style={{
          background:
            "linear-gradient(to left, var(--color-background), transparent)",
        }}
        aria-hidden="true"
      />

      {/* ── Header ── */}
      <div className="px-6 md:px-10 mb-16">
        <div className="max-w-7xl mx-auto flex flex-col md:flex-row md:items-end md:justify-between gap-6">
          <div>
            <motion.p
              initial={{ opacity: 0, y: 12 }}
              whileInView={{ opacity: 1, y: 0 }}
              viewport={{ once: true }}
              transition={{ duration: 0.5, ease }}
              className="text-xs font-medium tracking-widest uppercase text-primary mb-4"
            >
              Client love
            </motion.p>
            <motion.h2
              initial={{ opacity: 0, y: 16 }}
              whileInView={{ opacity: 1, y: 0 }}
              viewport={{ once: true }}
              transition={{ duration: 0.6, ease, delay: 0.08 }}
              className="text-[clamp(2rem,5vw,3.5rem)] font-black tracking-tighter text-foreground leading-[1.05]"
            >
              Don't take
              <br />
              <span className="text-primary">our word for it.</span>
            </motion.h2>
          </div>

          <motion.p
            initial={{ opacity: 0, y: 12 }}
            whileInView={{ opacity: 1, y: 0 }}
            viewport={{ once: true }}
            transition={{ duration: 0.6, ease, delay: 0.16 }}
            className="max-w-xs text-sm text-muted-foreground leading-relaxed md:text-right"
          >
            Real results from real clients — across industries, budgets, and timelines.
          </motion.p>
        </div>
      </div>

      {/* ── Marquee rows ── */}
      <motion.div
        initial={{ opacity: 0 }}
        whileInView={{ opacity: 1 }}
        viewport={{ once: true }}
        transition={{ duration: 0.8, ease }}
        className="flex flex-col gap-4"
      >
        <MarqueeRow items={ITEMS} />
        <MarqueeRow items={[...ITEMS].reverse()} reverse />
      </motion.div>

    </section>
  );
}