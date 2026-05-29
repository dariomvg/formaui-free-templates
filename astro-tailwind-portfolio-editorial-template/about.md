---
import Layout from "../layouts/Layout.astro";
import {siteData} from "../lib/data.ts";
const {about} = siteData; 
---


<Layout title="About — AR. STUDIO" description="Learn about AR. STUDIO, a creative studio specializing in visual identities, digital experiences, and editorial design. Discover our philosophy of blending clinical precision with expressive brutalism, our leadership under founder Adrián Rivera, and our curated selection of clients. Explore how we approach design with an editorial mindset to create impactful visual narratives." >
  <div class="grain-overlay"></div>
  <main class="pt-40">
<!-- Hero Section -->
<section class="px-margin-mobile md:px-margin-desktop mb-section-gap">
<h1 class="editorial-hero font-display-xl text-on-surface tracking-tighter uppercase italic">
                THE STUDIO
            </h1>
<div class="flex justify-end mt-4">
<span class="font-label-caps text-label-caps text-outline px-4 py-1 border border-outline-variant">EST. 2018</span>
</div>
</section>
<!-- Narrative Section -->
<section class="px-margin-mobile md:px-margin-desktop grid grid-cols-1 md:grid-cols-12 gap-gutter mb-section-gap">
<div class="md:col-span-3">
<h2 class="font-label-caps text-label-caps text-primary tracking-[0.2em]">PHILOSOPHY</h2>
</div>
<div class="md:col-span-8 md:col-start-5">
<p class="font-body-lg text-body-lg text-on-surface-variant leading-relaxed max-w-3xl">
                    {about.description}
                </p>
</div>
</section>
<!-- Leadership/Team Section -->
<section class="px-margin-mobile md:px-margin-desktop mb-section-gap border-t border-outline-variant pt-20">
<div class="grid grid-cols-1 md:grid-cols-12 gap-gutter items-end">
<div class="md:col-span-5 aspect-4/5 bg-surface-container-highest overflow-hidden">
<img alt="Adrián Rivera Portrait" class="w-full h-full object-cover filter grayscale contrast-125 brightness-75" data-alt="A high-contrast, moody black and white portrait of a man in his late 30s with sharp features and a focused gaze. The lighting is dramatic, with heavy shadows on one side of his face, evoking a sense of professional authority and mystery. He is wearing a dark, minimalist turtleneck that blends into the deep black background. The overall aesthetic is editorial and clinical, fitting for a premium creative studio director." 
src={about.image} />
</div>
<div class="md:col-span-6 md:col-start-7">
<span class="font-label-caps text-label-caps text-primary mb-4 block">{about.job}</span>
<h2 class="font-display-lg text-display-lg text-on-surface mb-8">{about.name}</h2>
<div class="space-y-6">
<p class="whitespace-pre-line font-body-md text-body-md text-on-surface-variant ">
{about.bio}
                        </p>

<div class="pt-4">
<a class="inline-flex items-center gap-4 group" href="#">
<span class="font-label-caps text-label-caps border-b border-primary pb-1 group-hover:text-primary transition-colors">READ INTERVIEW</span>
<img src="/icons/arrow-forward.svg" alt="arrow forward" width="20" height="20" class="group-hover:translate-x-1 transition-transform"/>
</a>
</div>
</div>
</div>
</div>
</section>
<!-- Selected Clients Section -->
<section class="px-margin-mobile md:px-margin-desktop mb-section-gap">
<div class="mb-10">
<span class="font-label-caps text-label-caps text-outline">03 / SELECTED CLIENTS</span>
</div>
<div class="grid grid-cols-2 md:grid-cols-4 gap-0 border-t border-l border-outline-variant">
    {about.clients.map(client => (
        <div class="p-8 border-r border-b border-outline-variant font-label-caps text-label-caps text-on-surface-variant hover:bg-surface-container transition-colors">
            {client}
        </div>
    ))}
</div>
</section>
</main>
</Layout>

<style>
  body {
    background-color: #0a0a0a;
    position: relative
    }
.grain-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    z-index: 9999;
    opacity: 0.05;
    background-image: url(https://lh3.googleusercontent.com/aida-public/AB6AXuDP_ukug6q6DT2kdpClbqz6YQctWiAyP8291Tcq9vUZyeqczJLlE66rGtq9HDSGzz0kA6vy9cCzdmfLkh0y4L_RQN-c8G27QrCgA1hxYpaUJpcJk4LzPAeAggv1z7MOExt25ewidIKmAiCWlPkPieYtu-pL0lxGOsX5KoJO_897Szo7AYFX2AveG-sy7A763E7s37rmUpvR5PcGbMe0WFxuO_dYLZV6Mp-pvouoMqONCh9BpTU8NyeuXHGLxLCcM--BeRNUByu_zmtu)
    }
.editorial-hero {
    font-size: clamp(72px, 12vw, 160px);
    line-height: 0.9
    }
</style>