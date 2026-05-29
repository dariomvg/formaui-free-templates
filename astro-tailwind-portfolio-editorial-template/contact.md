---
import Layout from "../layouts/Layout.astro";
import { siteData } from "../lib/data.ts";
---

<Layout title="Contact — AR. STUDIO" description="Get in touch with AR. STUDIO for collaborations, inquiries, or to discuss your next project. We are a creative studio specializing in visual identities, digital experiences, and editorial design. Contact us to explore how we can bring clinical precision and expressive brutalism to your brand." >
  <div class="grain-overlay"></div>
<div class="custom-cursor"></div>
<main class="min-h-screen pt-32">
<!-- Hero Section -->
<section class="px-margin-mobile md:px-margin-desktop pb-14 pt-30">
<h1 class="hero-text font-black font-display-xl text-on-surface tracking-tighter max-w-4xl">
                LET'S WORK
            </h1>
</section>
<!-- Content Grid -->
<section class="px-margin-mobile md:px-margin-desktop grid grid-cols-1 lg:grid-cols-12 gap-gutter mb-section-gap">
<!-- Left Column: Contact Form -->
<div class="lg:col-span-7 border-t border-outline-variant pt-10">
<form class="flex flex-col gap-12">
<div class="flex flex-col gap-4">
<label class="font-label-caps text-label-caps text-on-surface-variant">NAME</label>
<input class="bg-transparent border-0 border-b border-outline-variant focus:ring-0 focus:border-primary-container text-body-lg font-body-lg py-4 px-0 placeholder:text-surface-container-highest transition-all" placeholder="Your full name" type="text"/>
</div>
<div class="flex flex-col gap-4">
<label class="font-label-caps text-label-caps text-on-surface-variant">EMAIL</label>
<input class="bg-transparent border-0 border-b border-outline-variant focus:ring-0 focus:border-primary-container text-body-lg font-body-lg py-4 px-0 placeholder:text-surface-container-highest transition-all" placeholder="example@studio.com" type="email"/>
</div>
<div class="flex flex-col gap-4">
<label class="font-label-caps text-label-caps text-on-surface-variant">MESSAGE</label>
<textarea class="bg-transparent border-0 border-b border-outline-variant focus:ring-0 focus:border-primary-container text-body-lg font-body-lg py-4 px-0 placeholder:text-surface-container-highest transition-all resize-none" placeholder="Tell us about your project" rows="4"></textarea>
</div>
<div class="pt-8">
<button class="group flex items-center gap-4 bg-primary-container text-on-primary-container px-12 py-6 font-label-caps text-label-caps hover:bg-on-background hover:text-background transition-all duration-500 uppercase tracking-widest" type="submit">
                            Send Inquiry
                            <img src="/icons/arrow-forward.svg" alt="arrow forward" width="20" height="20" class="group-hover:translate-x-1 transition-transform"/>
</button>
</div>
</form>
</div>
<!-- Right Column: Studio Details -->
<div class="lg:col-span-4 lg:col-start-9 flex flex-col gap-20 border-t border-outline-variant pt-10">
<!-- Office -->
<div class="flex flex-col gap-6">
<h3 class="font-label-caps text-label-caps text-primary">OFFICE</h3>
<div class="flex flex-col">
<p class="font-body-lg text-body-lg">{siteData.author.ubication}</p>
</div>
</div>
<!-- Inquiries -->
<div class="flex flex-col gap-6">
<h3 class="font-label-caps text-label-caps text-primary">GENERAL INQUIRIES</h3>
<a class="font-body-lg text-body-lg hover:text-primary-container transition-colors" href="mailto:hello@arstudio.design">{siteData.author.email}</a>
</div>
<!-- Social -->
<div class="flex flex-col gap-6">
<h3 class="font-label-caps text-label-caps text-primary">SOCIAL</h3>
<ul class="flex flex-col gap-2">
    {siteData.social_links.map((social) => (
        <li>
            <a class="font-body-lg text-body-lg hover:text-primary-container transition-colors" href={social.url} target="_blank">
                {social.name}
            </a>
        </li>
    ))}
</ul>
</div>
<!-- Visual Anchor (Photography) -->
<div class="mt-8 border border-outline-variant p-4">
<img alt="Studio Environment" class="grayscale w-full aspect-4/5 object-cover" data-alt="A cinematic, high-contrast photograph of a minimalist architectural studio in Mexico City. The scene features clean lines, concrete walls, and large floor-to-ceiling windows letting in dramatic, soft side lighting. The interior is sparsely furnished with black designer chairs and a large wooden table. The overall mood is clinical, professional, and sophisticated, dominated by a deep monochromatic color palette with subtle warm highlights." src="https://lh3.googleusercontent.com/aida-public/AB6AXuAG7krHTwTdbZdqyMp7dzqlh_As0oz3E_QA3rWgY9-igMIA4cODDUnjj49x-d2xdQnhumQUTWTC5vkBtukUdR73FVfHbt-tzSSE3bbkb608M8jiQW2Gz5iXqqghDtcIimELWHG5PJ7bfpl5b_Kzr35NNKAFmm5GEBY12Wim_zmPecjnjMiw5OmZ4e-XiYzALi7eEPg20duBfxzkaaRUt5ZrqrYdEdnEoXpHNQ0EzezNhECQXIOy215PpnHXTvOy2jbEWcAsPinPiKim"/>
</div>
</div>
</section>
</main>
</Layout>

<style>
   body {
            background-color: #0a0a0a;
            position: relative;
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
            background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noiseFilter'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.65' numOctaves='3' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noiseFilter)'/%3E%3C/svg%3E");
        }
        .custom-cursor {
            width: 20px;
            height: 20px;
            border: 1px solid #c8b8a2;
            border-radius: 50%;
            position: fixed;
            pointer-events: none;
            z-index: 10000;
            transform: translate(-50%, -50%);
            display: none;
        }
        @media (min-width: 1024px) {
            .custom-cursor { display: block; }
        }
        .hero-text {
            font-size: clamp(72px, 12vw, 160px);
            line-height: 0.9;
        }
</style>


<script>
     // Simple cursor follower
        const cursor = document.querySelector<HTMLElement>('.custom-cursor');
      
            if(cursor) {
                 document.addEventListener('mousemove', e => {
            cursor.style.left = e.clientX + 'px';
            cursor.style.top = e.clientY + 'px';
        });

        // Hover effects for cursor
        document.querySelectorAll<HTMLElement>('a, button, input, textarea').forEach(el => {
            el.addEventListener('mouseenter', () => {
                cursor.style.width = '40px';
                cursor.style.height = '40px';
                cursor.style.backgroundColor = 'rgba(200, 184, 162, 0.1)';
            });
            el.addEventListener('mouseleave', () => {
                cursor.style.width = '20px';
                cursor.style.height = '20px';
                cursor.style.backgroundColor = 'transparent';
            });
        });
            }
        
</script>