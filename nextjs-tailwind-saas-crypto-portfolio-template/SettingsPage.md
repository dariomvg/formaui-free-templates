"use client";

import { useState } from "react";
import {
  Eye,
  EyeOff,
  Plus,
  Trash2,
  ShieldCheck,
  Smartphone,
  Globe,
} from "lucide-react";
import {
  Card,
  CardHeader,
  CardTitle,
  CardDescription,
  CardContent,
  CardFooter,
} from "@/components/ui/card";
import {
  Tabs,
  TabsList,
  TabsTrigger,
  TabsContent,
} from "@/components/ui/tabs";
import { Input } from "@/components/ui/input";
import { Label } from "@/components/ui/label";
import { Button } from "@/components/ui/button";
import { Switch } from "@/components/ui/switch";
import { Badge } from "@/components/ui/badge";
import { Avatar, AvatarFallback } from "@/components/ui/avatar";
import {
  Select,
  SelectContent,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from "@/components/ui/select";
import {
  Dialog,
  DialogContent,
  DialogHeader,
  DialogTitle,
  DialogDescription,
  DialogFooter,
  DialogTrigger,
  DialogClose,
} from "@/components/ui/dialog";
import { Separator } from "@/components/ui/separator";

// ─────────────────────────────────────────────
// CURRENCIES
// ─────────────────────────────────────────────

const currencies = [
  { value: "usd", label: "USD — US Dollar" },
  { value: "eur", label: "EUR — Euro" },
  { value: "gbp", label: "GBP — British Pound" },
  { value: "ars", label: "ARS — Argentine Peso" },
  { value: "btc", label: "BTC — Bitcoin" },
];

// ─────────────────────────────────────────────
// ACCOUNT TAB
// ─────────────────────────────────────────────

function AccountTab() {
  return (
    <div className="flex flex-col gap-6">
      <Card className="bg-card/50 border-border/60">
        <CardHeader>
          <CardTitle>Profile</CardTitle>
          <CardDescription>Update your personal information.</CardDescription>
        </CardHeader>
        <CardContent className="flex flex-col gap-6">
          {/* Avatar */}
          <div className="flex items-center gap-4">
            <Avatar className="size-16 border border-border/60">
              <AvatarFallback className="bg-primary/10 text-primary text-lg font-semibold">
                DR
              </AvatarFallback>
            </Avatar>
            <div className="flex flex-col gap-2">
              <Button variant="outline" size="sm">
                Change avatar
              </Button>
              <p className="text-xs text-muted-foreground">
                JPG, PNG or GIF. Max 2MB.
              </p>
            </div>
          </div>

          <Separator />

          {/* Fields */}
          <div className="grid grid-cols-1 sm:grid-cols-2 gap-4">
            <div className="flex flex-col gap-2">
              <Label htmlFor="name">Display name</Label>
              <Input id="name" defaultValue="Dari" />
            </div>
            <div className="flex flex-col gap-2">
              <Label htmlFor="email">Email address</Label>
              <Input id="email" type="email" defaultValue="dari@cryptolens.app" />
            </div>
          </div>
        </CardContent>
        <CardFooter className="justify-end border-t border-border/60 pt-4">
          <Button>Save changes</Button>
        </CardFooter>
      </Card>

      {/* Preferences */}
      <Card className="bg-card/50 border-border/60">
        <CardHeader>
          <CardTitle>Preferences</CardTitle>
          <CardDescription>Customize how data is displayed across CryptoLens.</CardDescription>
        </CardHeader>
        <CardContent className="flex flex-col gap-6">
          <div className="grid grid-cols-1 sm:grid-cols-2 gap-4">
            <div className="flex flex-col gap-2">
              <Label htmlFor="currency">Base currency</Label>
              <Select defaultValue="usd">
                <SelectTrigger id="currency">
                  <SelectValue placeholder="Select currency" />
                </SelectTrigger>
                <SelectContent>
                  {currencies.map((c) => (
                    <SelectItem key={c.value} value={c.value}>
                      {c.label}
                    </SelectItem>
                  ))}
                </SelectContent>
              </Select>
            </div>

            <div className="flex flex-col gap-2">
              <Label htmlFor="cost-basis">Cost basis method</Label>
              <Select defaultValue="fifo">
                <SelectTrigger id="cost-basis">
                  <SelectValue placeholder="Select method" />
                </SelectTrigger>
                <SelectContent>
                  <SelectItem value="fifo">FIFO — First in, first out</SelectItem>
                  <SelectItem value="lifo">LIFO — Last in, first out</SelectItem>
                  <SelectItem value="avg">Weighted average</SelectItem>
                </SelectContent>
              </Select>
            </div>
          </div>

          <div className="flex items-center justify-between rounded-lg border border-border/60 bg-muted/30 px-4 py-3">
            <div className="flex flex-col gap-0.5">
              <span className="text-sm font-medium text-foreground">
                Hide small balances
              </span>
              <span className="text-xs text-muted-foreground">
                Assets worth less than $1 won&apos;t appear in your portfolio.
              </span>
            </div>
            <Switch defaultChecked />
          </div>
        </CardContent>
        <CardFooter className="justify-end border-t border-border/60 pt-4">
          <Button>Save changes</Button>
        </CardFooter>
      </Card>
    </div>
  );
}

// ─────────────────────────────────────────────
// NOTIFICATIONS TAB
// ─────────────────────────────────────────────

interface NotificationRow {
  id: string;
  label: string;
  description: string;
  defaultChecked: boolean;
}

const notificationGroups: { heading: string; items: NotificationRow[] }[] = [
  {
    heading: "Price alerts",
    items: [
      {
        id: "price-alerts",
        label: "Watchlist price alerts",
        description: "Get notified when an asset crosses your target price.",
        defaultChecked: true,
      },
      {
        id: "daily-summary",
        label: "Daily portfolio summary",
        description: "A recap of your portfolio performance, every morning.",
        defaultChecked: true,
      },
    ],
  },
  {
    heading: "Account",
    items: [
      {
        id: "security-alerts",
        label: "Security alerts",
        description: "Sign-ins from new devices or locations.",
        defaultChecked: true,
      },
      {
        id: "product-updates",
        label: "Product updates",
        description: "New features and improvements to CryptoLens.",
        defaultChecked: false,
      },
      {
        id: "marketing",
        label: "Marketing emails",
        description: "Tips, offers, and occasional promotions.",
        defaultChecked: false,
      },
    ],
  },
];

function NotificationsTab() {
  return (
    <div className="flex flex-col gap-6">
      {notificationGroups.map((group) => (
        <Card key={group.heading} className="bg-card/50 border-border/60">
          <CardHeader>
            <CardTitle>{group.heading}</CardTitle>
          </CardHeader>
          <CardContent className="flex flex-col gap-3">
            {group.items.map((item) => (
              <div
                key={item.id}
                className="flex items-center justify-between rounded-lg border border-border/60 bg-muted/30 px-4 py-3"
              >
                <div className="flex flex-col gap-0.5 pr-4">
                  <Label htmlFor={item.id} className="text-sm font-medium text-foreground cursor-pointer">
                    {item.label}
                  </Label>
                  <span className="text-xs text-muted-foreground">
                    {item.description}
                  </span>
                </div>
                <Switch id={item.id} defaultChecked={item.defaultChecked} />
              </div>
            ))}
          </CardContent>
        </Card>
      ))}
    </div>
  );
}

// ─────────────────────────────────────────────
// API KEY ROW
// ─────────────────────────────────────────────

interface ApiKeyConnection {
  id: string;
  exchange: string;
  label: string;
  permissions: string;
  status: "connected" | "expired";
  lastSync: string;
}

const connections: ApiKeyConnection[] = [
  { id: "1", exchange: "Binance", label: "Main account", permissions: "Read-only", status: "connected", lastSync: "2 min ago" },
  { id: "2", exchange: "Coinbase", label: "Trading account", permissions: "Read-only", status: "connected", lastSync: "14 min ago" },
  { id: "3", exchange: "Kraken", label: "Savings", permissions: "Read-only", status: "expired", lastSync: "3 days ago" },
];

function ConnectionRow({ connection }: { connection: ApiKeyConnection }) {
  return (
    <div className="flex items-center justify-between rounded-lg border border-border/60 bg-muted/30 px-4 py-3">
      <div className="flex items-center gap-3">
        <div className="flex items-center justify-center size-9 rounded-lg border border-border/60 bg-muted/50 shrink-0">
          <Globe className="size-4 text-muted-foreground" strokeWidth={1.75} />
        </div>
        <div className="flex flex-col gap-0.5">
          <div className="flex items-center gap-2">
            <span className="text-sm font-medium text-foreground">
              {connection.exchange}
            </span>
            <Badge
              variant="outline"
              className="text-[10px] px-1.5 py-0 h-4 border-border/60 text-muted-foreground"
            >
              {connection.permissions}
            </Badge>
          </div>
          <span className="text-xs text-muted-foreground">
            {connection.label} · Synced {connection.lastSync}
          </span>
        </div>
      </div>

      <div className="flex items-center gap-3">
        {connection.status === "connected" ? (
          <Badge className="bg-primary/10 text-primary border-0 text-xs">
            Connected
          </Badge>
        ) : (
          <Badge className="bg-destructive/10 text-destructive border-0 text-xs">
            Expired
          </Badge>
        )}
        <Button variant="ghost" size="icon" className="size-8 text-muted-foreground hover:text-destructive" aria-label={`Remove ${connection.exchange} connection`}>
          <Trash2 className="size-4" />
        </Button>
      </div>
    </div>
  );
}

function ConnectExchangeDialog() {
  const [showKey, setShowKey] = useState(false);

  return (
    <Dialog>
      <DialogTrigger asChild>
        <Button size="sm" className="gap-1.5">
          <Plus className="size-4" />
          Connect exchange
        </Button>
      </DialogTrigger>
      <DialogContent className="sm:max-w-md">
        <DialogHeader>
          <DialogTitle>Connect an exchange</DialogTitle>
          <DialogDescription>
            Add a read-only API key. CryptoLens can never withdraw or trade on your behalf.
          </DialogDescription>
        </DialogHeader>

        <div className="flex flex-col gap-4 py-2">
          <div className="flex flex-col gap-2">
            <Label htmlFor="exchange">Exchange</Label>
            <Select defaultValue="binance">
              <SelectTrigger id="exchange">
                <SelectValue placeholder="Select exchange" />
              </SelectTrigger>
              <SelectContent>
                <SelectItem value="binance">Binance</SelectItem>
                <SelectItem value="coinbase">Coinbase</SelectItem>
                <SelectItem value="kraken">Kraken</SelectItem>
                <SelectItem value="bybit">Bybit</SelectItem>
                <SelectItem value="okx">OKX</SelectItem>
              </SelectContent>
            </Select>
          </div>

          <div className="flex flex-col gap-2">
            <Label htmlFor="api-key">API key</Label>
            <Input id="api-key" placeholder="Enter your API key" />
          </div>

          <div className="flex flex-col gap-2">
            <Label htmlFor="api-secret">API secret</Label>
            <div className="relative">
              <Input
                id="api-secret"
                type={showKey ? "text" : "password"}
                placeholder="Enter your API secret"
                className="pr-10"
              />
              <button
                type="button"
                onClick={() => setShowKey((v) => !v)}
                className="absolute right-3 top-1/2 -translate-y-1/2 text-muted-foreground hover:text-foreground transition-colors duration-150"
                aria-label={showKey ? "Hide secret" : "Show secret"}
              >
                {showKey ? <EyeOff className="size-4" /> : <Eye className="size-4" />}
              </button>
            </div>
          </div>

          <div className="flex items-start gap-2.5 rounded-lg border border-primary/20 bg-primary/5 px-3 py-2.5">
            <ShieldCheck className="size-4 text-primary mt-0.5 shrink-0" />
            <p className="text-xs text-muted-foreground leading-relaxed">
              Make sure to enable only <span className="text-foreground font-medium">read-only</span> permissions
              when generating your API key. We&apos;ll verify this on connection.
            </p>
          </div>
        </div>

        <DialogFooter>
          <DialogClose asChild>
            <Button type="button" variant="outline">
              Cancel
            </Button>
          </DialogClose>
          <DialogClose asChild>
            <Button type="button">Connect</Button>
          </DialogClose>
        </DialogFooter>
      </DialogContent>
    </Dialog>
  );
}

function ApiKeysTab() {
  return (
    <div className="flex flex-col gap-6">
      <Card className="bg-card/50 border-border/60">
        <CardHeader className="flex flex-row items-center justify-between">
          <div>
            <CardTitle>Connected exchanges</CardTitle>
            <CardDescription>
              Manage read-only API connections for portfolio sync.
            </CardDescription>
          </div>
          <ConnectExchangeDialog />
        </CardHeader>
        <CardContent className="flex flex-col gap-3">
          {connections.map((connection) => (
            <ConnectionRow key={connection.id} connection={connection} />
          ))}
        </CardContent>
      </Card>
    </div>
  );
}

// ─────────────────────────────────────────────
// SECURITY TAB
// ─────────────────────────────────────────────

function SecurityTab() {
  return (
    <div className="flex flex-col gap-6">
      <Card className="bg-card/50 border-border/60">
        <CardHeader>
          <CardTitle>Two-factor authentication</CardTitle>
          <CardDescription>
            Add an extra layer of security to your account.
          </CardDescription>
        </CardHeader>
        <CardContent>
          <div className="flex items-center justify-between rounded-lg border border-border/60 bg-muted/30 px-4 py-3">
            <div className="flex items-center gap-3">
              <div className="flex items-center justify-center size-9 rounded-lg border border-border/60 bg-muted/50 shrink-0">
                <Smartphone className="size-4 text-muted-foreground" strokeWidth={1.75} />
              </div>
              <div className="flex flex-col gap-0.5">
                <span className="text-sm font-medium text-foreground">
                  Authenticator app
                </span>
                <span className="text-xs text-muted-foreground">
                  Not enabled
                </span>
              </div>
            </div>
            <Button variant="outline" size="sm">
              Enable
            </Button>
          </div>
        </CardContent>
      </Card>

      <Card className="bg-card/50 border-border/60">
        <CardHeader>
          <CardTitle>Password</CardTitle>
          <CardDescription>Change your account password.</CardDescription>
        </CardHeader>
        <CardContent className="flex flex-col gap-4">
          <div className="grid grid-cols-1 sm:grid-cols-2 gap-4">
            <div className="flex flex-col gap-2">
              <Label htmlFor="current-password">Current password</Label>
              <Input id="current-password" type="password" placeholder="••••••••" />
            </div>
            <div className="flex flex-col gap-2">
              <Label htmlFor="new-password">New password</Label>
              <Input id="new-password" type="password" placeholder="••••••••" />
            </div>
          </div>
        </CardContent>
        <CardFooter className="justify-end border-t border-border/60 pt-4">
          <Button>Update password</Button>
        </CardFooter>
      </Card>

      <Card className="border-destructive/30 bg-destructive/5">
        <CardHeader>
          <CardTitle className="text-destructive">Danger zone</CardTitle>
          <CardDescription>
            Permanently delete your account and all associated data.
          </CardDescription>
        </CardHeader>
        <CardFooter className="justify-end border-t border-destructive/20 pt-4">
          <Button variant="outline" className="border-destructive/40 text-destructive hover:bg-destructive/10 hover:text-destructive">
            Delete account
          </Button>
        </CardFooter>
      </Card>
    </div>
  );
}

// ─────────────────────────────────────────────
// SETTINGS PAGE
// ─────────────────────────────────────────────

export default function SettingsPage() {
  return (
    <div className="flex flex-col gap-6 max-w-4xl">

      {/* Header */}
      <div className="flex flex-col gap-1">
        <h1 className="text-2xl font-bold tracking-tight text-foreground">
          Settings
        </h1>
        <p className="text-sm text-muted-foreground">
          Manage your account, preferences, and connected exchanges.
        </p>
      </div>

      <Tabs defaultValue="account">
        <TabsList>
          <TabsTrigger value="account">Account</TabsTrigger>
          <TabsTrigger value="notifications">Notifications</TabsTrigger>
          <TabsTrigger value="api-keys">Exchanges</TabsTrigger>
          <TabsTrigger value="security">Security</TabsTrigger>
        </TabsList>

        <TabsContent value="account" className="mt-6">
          <AccountTab />
        </TabsContent>

        <TabsContent value="notifications" className="mt-6">
          <NotificationsTab />
        </TabsContent>

        <TabsContent value="api-keys" className="mt-6">
          <ApiKeysTab />
        </TabsContent>

        <TabsContent value="security" className="mt-6">
          <SecurityTab />
        </TabsContent>
      </Tabs>
    </div>
  );
}
