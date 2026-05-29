"use client"

import { useState } from "react"
import { Check, X } from "lucide-react"
import { Button } from "@/components/ui/button"
import { cn } from "@/lib/utils"
import {plans} from "@/lib/config"

export function Pricing() {
  const [annual, setAnnual] = useState(false)

  return (
    <section className="w-full py-24 px-4 md:px-6">
      <div className="max-w-5xl mx-auto">

        {/* Header */}
        <div className="text-center mb-12">
          <span className="text-xs font-medium uppercase tracking-widest text-primary border border-primary/20 bg-primary/10 px-3 py-1 rounded-full">
            Pricing
          </span>
          <h2 className="text-4xl font-semibold mt-4 mb-3 text-foreground">
            Simple pricing, serious results.
          </h2>
          <p className="text-muted-foreground text-lg max-w-md mx-auto mb-7">
            No hidden fees. No surprise invoices. Cancel any time.
          </p>

          {/* Toggle */}
          <div className="inline-flex items-center gap-0 bg-muted border border-border rounded-lg p-1">
            <button
              onClick={() => setAnnual(false)}
              className={cn(
                "text-sm font-medium px-4 py-1.5 rounded-md transition-all",
                !annual
                  ? "bg-background text-foreground border border-border shadow-sm"
                  : "text-muted-foreground"
              )}
            >
              Monthly
            </button>
            <button
              onClick={() => setAnnual(true)}
              className={cn(
                "text-sm font-medium px-4 py-1.5 rounded-md transition-all flex items-center gap-1.5",
                annual
                  ? "bg-background text-foreground border border-border shadow-sm"
                  : "text-muted-foreground"
              )}
            >
              Annual
              <span className="text-xs text-green-400 font-medium">−20%</span>
            </button>
          </div>
        </div>

        {/* Cards */}
        <div className="grid grid-cols-1 md:grid-cols-3 divide-y md:divide-y-0 md:divide-x divide-border border border-border rounded-xl overflow-hidden">
          {plans.map((plan) => (
            <div
              key={plan.name}
              className={cn(
                "bg-card p-8 flex flex-col relative",
                plan.featured && "outline-2 outline-primary/40 z-10"
              )}
            >
              {/* Badge most popular */}
              {plan.featured && (
                <div className="absolute -top-px left-1/2 -translate-x-1/2">
                  <span className="text-xs font-medium text-primary bg-primary/10 border border-primary/20 px-3 py-0.5 rounded-b-lg whitespace-nowrap">
                    Most popular
                  </span>
                </div>
              )}

              {/* Price */}
              <div className="mb-8">
                <p className={cn(
                  "text-xs font-medium uppercase tracking-widest mb-3",
                  plan.featured ? "text-primary" : "text-muted-foreground"
                )}>
                  {plan.name}
                </p>

                <div className="flex items-baseline gap-1 mb-1.5">
                  {plan.price.monthly === null ? (
                    <span className="text-4xl font-medium text-foreground leading-none">Custom</span>
                  ) : plan.price.monthly === 0 ? (
                    <>
                      <span className="text-4xl font-medium text-foreground leading-none">$0</span>
                      <span className="text-sm text-muted-foreground">/mo</span>
                    </>
                  ) : (
                    <>
                      <span className="text-4xl font-medium text-foreground leading-none">
                        ${annual ? plan.price.annual : plan.price.monthly}
                      </span>
                      <span className="text-sm text-muted-foreground">/mo</span>
                    </>
                  )}
                </div>

                <p className="text-xs text-muted-foreground">
                  {plan.price.monthly !== null && plan.price.monthly !== 0
                    ? annual
                      ? plan.annualNote
                      : plan.monthlyNote
                    : plan.desc}
                </p>
              </div>

              {/* Features */}
              <ul className="flex-1 flex flex-col gap-2.5 mb-8">
                {plan.features.map((f) => (
                  <li key={f.text} className="flex items-start gap-2.5">
                    {f.included ? (
                      <Check className="w-3.5 h-3.5 mt-0.5 text-green-400 shrink-0" />
                    ) : (
                      <X className="w-3.5 h-3.5 mt-0.5 text-border shrink-0" />
                    )}
                    <span className={cn(
                      "text-sm",
                      f.included ? "text-muted-foreground" : "text-muted-foreground/40"
                    )}>
                      {f.text}
                    </span>
                  </li>
                ))}
              </ul>

              {/* CTA */}
              <Button
                variant={plan.featured ? "default" : "outline"}
                className="w-full"
              >
                {plan.cta}
              </Button>
            </div>
          ))}
        </div>

        <p className="text-center text-xs text-muted-foreground mt-5">
          All plans include a 14-day free trial. No credit card required.
        </p>

      </div>
    </section>
  )
}