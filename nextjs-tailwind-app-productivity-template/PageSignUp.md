"use client";

import { useState } from "react";
import Link from "next/link";
import {
  Eye,
  EyeOff,
  ArrowRight,
} from "lucide-react";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Label } from "@/components/ui/label";
import { Separator } from "@/components/ui/separator";
import { cn } from "@/lib/utils";
import {  highlightsSignup as highlights, quotesSignup as quotes } from "@/lib/config";

export default function SignupPage() {
  const [showPassword, setShowPassword] = useState(false);
  const [name, setName] = useState("");
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  return (
    <div className="min-h-screen bg-background grid grid-cols-1 lg:grid-cols-2">
      {/* ── Left panel — decorative ── */}
      <div className="relative flex items-center justify-center px-6 py-16">
        <div className="relative w-full max-w-sm flex flex-col gap-8">
          {/* Mobile logo */}
          <div className="flex flex-col gap-1 lg:hidden">
            <Link
              href="/"
              className="text-foreground font-semibold text-lg tracking-tight w-fit">
              FocusKit
            </Link>
          </div>

          {/* Header */}
          <div className="flex flex-col gap-1">
            <h1 className="text-2xl font-black text-foreground">
              Create a account
            </h1>
            <p className="text-xs font-mono text-muted-foreground">
              Free forever on the basic plan.
            </p>
          </div>

          {/* Card */}
          <div className="rounded-xl border border-border bg-transparent p-8 flex flex-col gap-6">
            {/* Google */}
            <Button
              variant="outline"
              className="w-full h-10 text-sm font-medium gap-3 border-border text-foreground hover:bg-secondary">
              <svg viewBox="0 0 24 24" className="w-4 h-4 shrink-0" aria-hidden>
                <path
                  d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z"
                  fill="#4285F4"
                />
                <path
                  d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z"
                  fill="#34A853"
                />
                <path
                  d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.93l3.66-2.84z"
                  fill="#FBBC05"
                />
                <path
                  d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z"
                  fill="#EA4335"
                />
              </svg>
              Continue with Google
            </Button>

            {/* Divider */}
            <div className="flex items-center gap-3">
              <Separator className="flex-1 bg-border" />
              <span className="text-xs font-mono text-muted-foreground">o</span>
              <Separator className="flex-1 bg-border" />
            </div>

            {/* Form */}
            <form
              className="flex flex-col gap-4"
              onSubmit={(e) => e.preventDefault()}>
              <div className="flex flex-col gap-1.5">
                <Label
                  htmlFor="name"
                  className="text-xs font-mono text-muted-foreground uppercase tracking-widest">
                  Name
                </Label>
                <Input
                  id="name"
                  type="text"
                  placeholder="Your name"
                  value={name}
                  onChange={(e) => setName(e.target.value)}
                  className="h-10 bg-background border-border text-sm font-mono placeholder:text-muted-foreground/40 focus-visible:ring-primary/30"
                />
              </div>

              <div className="flex flex-col gap-1.5">
                <Label
                  htmlFor="email"
                  className="text-xs font-mono text-muted-foreground uppercase tracking-widest">
                  Email
                </Label>
                <Input
                  id="email"
                  type="email"
                  placeholder="your@email.com"
                  value={email}
                  onChange={(e) => setEmail(e.target.value)}
                  className="h-10 bg-background border-border text-sm font-mono placeholder:text-muted-foreground/40 focus-visible:ring-primary/30"
                />
              </div>

              <div className="flex flex-col gap-1.5">
                <Label
                  htmlFor="password"
                  className="text-xs font-mono text-muted-foreground uppercase tracking-widest">
                  Password
                </Label>
                <div className="relative">
                  <Input
                    id="password"
                    type={showPassword ? "text" : "password"}
                    placeholder="Minimum 8 characters"
                    value={password}
                    onChange={(e) => setPassword(e.target.value)}
                    className="h-10 bg-background border-border text-sm font-mono placeholder:text-muted-foreground/40 focus-visible:ring-primary/30 pr-10"
                  />
                  <button
                    type="button"
                    onClick={() => setShowPassword(!showPassword)}
                    className="absolute right-3 top-1/2 -translate-y-1/2 text-muted-foreground hover:text-foreground transition-colors">
                    {showPassword ? <EyeOff size={14} /> : <Eye size={14} />}
                  </button>
                </div>
              </div>

              <Button
                type="submit"
                size="sm"
                className={cn(
                  "w-full h-10 text-sm font-mono gap-2 group mt-1",
                  "bg-primary text-primary-foreground",
                )}>
                Create free account
                <ArrowRight
                  size={13}
                  className="transition-transform group-hover:translate-x-0.5"
                />
              </Button>
            </form>

            {/* Terms */}
            <p className="text-[11px] font-mono text-muted-foreground/60 text-center leading-relaxed">
              By registering, you agree to the{" "}
              <Link
                href="/terms"
                className="underline underline-offset-2 hover:text-muted-foreground transition-colors">
                Terms of Service
              </Link>{" "}
              and the{" "}
              <Link
                href="/privacy"
                className="underline underline-offset-2 hover:text-muted-foreground transition-colors">
                Privacy Policy
              </Link>
              .
            </p>
          </div>

          {/* Login link */}
          <p className="text-center text-xs font-mono text-muted-foreground">
            Do you have an account?{" "}
            <Link
              href="/auth/login"
              className="text-foreground underline underline-offset-4 hover:text-primary transition-colors">
              Sign in
            </Link>
          </p>
        </div>
      </div>

      {/* ── Right panel — form ── */}
      <div className="relative hidden lg:flex flex-col justify-between p-14 bg-card border-r border-border overflow-hidden">
        {/* Top — logo */}
        <div className="relative">
          <Link
            href="/"
            className="text-foreground font-semibold text-lg tracking-tight">
            FocusKit
          </Link>
        </div>

        {/* Middle — headline + highlights */}
        <div className="relative flex flex-col gap-10">
          <h2 className="text-4xl font-black leading-tight text-foreground">
            Everything you need
            <br />
            to perform better
            <br />
            <span className="text-primary">In one place.</span>
          </h2>

          <ul className="flex flex-col gap-4">
            {highlights.map((h) => {
              const Icon = h.icon;
              return (
                <li key={h.text} className="flex items-center gap-3">
                  <span className="w-7 h-7 rounded-md border border-primary/20 bg-primary/10 flex items-center justify-center shrink-0">
                    <Icon size={13} className="text-primary" />
                  </span>
                  <span className="text-sm text-muted-foreground">
                    {h.text}
                  </span>
                </li>
              );
            })}
          </ul>
        </div>

        {/* Bottom — quote */}
        <div className="relative border-t border-border pt-8">
          <p className="text-sm text-foreground/80 leading-relaxed mb-4">
            "{quotes[1].text}"
          </p>
          <div className="flex items-center gap-3">
            <div className="w-7 h-7 rounded-full bg-primary/10 border border-primary/20 flex items-center justify-center">
              <span className="text-[10px] font-mono text-primary font-bold">
                {quotes[1].author.charAt(0)}
              </span>
            </div>
            <div>
              <p className="text-xs font-semibold text-foreground">
                {quotes[1].author}
              </p>
              <p className="text-xs font-mono text-muted-foreground">
                {quotes[1].role}
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
}
