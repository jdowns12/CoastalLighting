<script>
	import site from '\$lib/data/site.json';
	import { onMount } from 'svelte';

	let scrolled = false;
	let mobileMenuOpen = false;

	onMount(() => {
		const handleScroll = () => {
			scrolled = window.scrollY > 50;
		};
		window.addEventListener('scroll', handleScroll);
		return () => window.removeEventListener('scroll', handleScroll);
	});
</script>

<svelte:head>
	<title>{site.name} | {site.tagline}</title>
	<meta name="description" content="{site.tagline}. Professional event lighting in {site.location}." />
	<link rel="preconnect" href="https://fonts.googleapis.com">
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
	<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;500;600;700&family=Inter:wght@300;400;500;600&display=swap" rel="stylesheet">
</svelte:head>

<div class="page-wrapper">
	<header class:scrolled>
		<nav>
			<a href="/" class="logo">
				<span class="logo-icon">✦</span>
				<span class="logo-text">{site.name}</span>
			</a>
			<button class="mobile-toggle" on:click={() => mobileMenuOpen = !mobileMenuOpen} aria-label="Menu">
				<span class:open={mobileMenuOpen}></span>
			</button>
			<div class="nav-links" class:open={mobileMenuOpen}>
				<a href="#services" on:click={() => mobileMenuOpen = false}>Services</a>
				<a href="#gallery" on:click={() => mobileMenuOpen = false}>Gallery</a>
				<a href="#testimonials" on:click={() => mobileMenuOpen = false}>Reviews</a>
				<a href="#contact" class="nav-cta" on:click={() => mobileMenuOpen = false}>Get Quote</a>
			</div>
		</nav>
	</header>

	<main>
		<slot />
	</main>

	<footer>
		<div class="footer-glow"></div>
		<div class="footer-content">
			<div class="footer-brand">
				<span class="logo-icon large">✦</span>
				<h2>{site.name}</h2>
				<p class="location">{site.location}</p>
			</div>
			<div class="footer-contact">
				<a href="tel:{site.phone}" class="phone">{site.phone}</a>
				<a href="mailto:{site.email}">{site.email}</a>
			</div>
			<div class="footer-social">
				<a href={site.social.facebook} target="_blank" rel="noopener" aria-label="Facebook">
					<svg viewBox="0 0 24 24" fill="currentColor"><path d="M24 12.073c0-6.627-5.373-12-12-12s-12 5.373-12 12c0 5.99 4.388 10.954 10.125 11.854v-8.385H7.078v-3.47h3.047V9.43c0-3.007 1.792-4.669 4.533-4.669 1.312 0 2.686.235 2.686.235v2.953H15.83c-1.491 0-1.956.925-1.956 1.874v2.25h3.328l-.532 3.47h-2.796v8.385C19.612 23.027 24 18.062 24 12.073z"/></svg>
				</a>
				<a href={site.social.instagram} target="_blank" rel="noopener" aria-label="Instagram">
					<svg viewBox="0 0 24 24" fill="currentColor"><path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zM12 0C8.741 0 8.333.014 7.053.072 2.695.272.273 2.69.073 7.052.014 8.333 0 8.741 0 12c0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98C8.333 23.986 8.741 24 12 24c3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98C15.668.014 15.259 0 12 0zm0 5.838a6.162 6.162 0 100 12.324 6.162 6.162 0 000-12.324zM12 16a4 4 0 110-8 4 4 0 010 8zm6.406-11.845a1.44 1.44 0 100 2.881 1.44 1.44 0 000-2.881z"/></svg>
				</a>
			</div>
			<p class="copyright">&copy; {site.footer.copyright}</p>
		</div>
	</footer>
</div>

<style>
	:global(*) {
		margin: 0;
		padding: 0;
		box-sizing: border-box;
	}

	:global(body) {
		font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
		line-height: 1.6;
		color: #e8e4de;
		background: #0a0a0a;
		overflow-x: hidden;
	}

	:global(html) {
		scroll-behavior: smooth;
	}

	:global(h1, h2, h3, h4) {
		font-family: 'Playfair Display', Georgia, serif;
		font-weight: 500;
	}

	.page-wrapper {
		min-height: 100vh;
	}

	/* Header */
	header {
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		z-index: 100;
		padding: 1.25rem 2rem;
		transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
	}

	header.scrolled {
		background: rgba(10, 10, 10, 0.95);
		backdrop-filter: blur(20px);
		padding: 0.75rem 2rem;
		box-shadow: 0 4px 30px rgba(0, 0, 0, 0.3);
	}

	nav {
		display: flex;
		justify-content: space-between;
		align-items: center;
		max-width: 1400px;
		margin: 0 auto;
	}

	.logo {
		display: flex;
		align-items: center;
		gap: 0.75rem;
		text-decoration: none;
		color: #e8e4de;
	}

	.logo-icon {
		font-size: 1.5rem;
		color: #d4a853;
		text-shadow: 0 0 20px rgba(212, 168, 83, 0.5);
		animation: pulse 3s ease-in-out infinite;
	}

	.logo-icon.large {
		font-size: 3rem;
	}

	@keyframes pulse {
		0%, 100% { opacity: 1; }
		50% { opacity: 0.7; }
	}

	.logo-text {
		font-family: 'Playfair Display', serif;
		font-size: 1.2rem;
		font-weight: 500;
		letter-spacing: 0.5px;
	}

	.nav-links {
		display: flex;
		align-items: center;
		gap: 2.5rem;
	}

	.nav-links a {
		color: #a8a8a8;
		text-decoration: none;
		font-size: 0.9rem;
		font-weight: 400;
		letter-spacing: 0.5px;
		transition: color 0.3s ease;
		position: relative;
	}

	.nav-links a:hover {
		color: #e8e4de;
	}

	.nav-links a::after {
		content: '';
		position: absolute;
		bottom: -4px;
		left: 0;
		width: 0;
		height: 1px;
		background: linear-gradient(90deg, #d4a853, transparent);
		transition: width 0.3s ease;
	}

	.nav-links a:hover::after {
		width: 100%;
	}

	.nav-cta {
		padding: 0.6rem 1.5rem !important;
		background: linear-gradient(135deg, #d4a853 0%, #b8923f 100%);
		color: #0a0a0a !important;
		border-radius: 4px;
		font-weight: 500 !important;
	}

	.nav-cta::after {
		display: none !important;
	}

	.nav-cta:hover {
		transform: translateY(-2px);
		box-shadow: 0 8px 25px rgba(212, 168, 83, 0.3);
	}

	/* Mobile menu */
	.mobile-toggle {
		display: none;
		background: none;
		border: none;
		width: 30px;
		height: 20px;
		position: relative;
		cursor: pointer;
		z-index: 101;
	}

	.mobile-toggle span,
	.mobile-toggle span::before,
	.mobile-toggle span::after {
		display: block;
		position: absolute;
		width: 100%;
		height: 2px;
		background: #e8e4de;
		transition: all 0.3s ease;
	}

	.mobile-toggle span {
		top: 50%;
		transform: translateY(-50%);
	}

	.mobile-toggle span::before {
		content: '';
		top: -8px;
	}

	.mobile-toggle span::after {
		content: '';
		top: 8px;
	}

	.mobile-toggle span.open {
		background: transparent;
	}

	.mobile-toggle span.open::before {
		top: 0;
		transform: rotate(45deg);
	}

	.mobile-toggle span.open::after {
		top: 0;
		transform: rotate(-45deg);
	}

	/* Footer */
	footer {
		position: relative;
		background: #050505;
		padding: 5rem 2rem 2rem;
		text-align: center;
		overflow: hidden;
	}

	.footer-glow {
		position: absolute;
		top: -100px;
		left: 50%;
		transform: translateX(-50%);
		width: 400px;
		height: 200px;
		background: radial-gradient(ellipse, rgba(212, 168, 83, 0.15) 0%, transparent 70%);
		pointer-events: none;
	}

	.footer-content {
		position: relative;
		max-width: 1200px;
		margin: 0 auto;
	}

	.footer-brand {
		margin-bottom: 2rem;
	}

	.footer-brand h2 {
		font-size: 2rem;
		color: #e8e4de;
		margin: 0.5rem 0;
	}

	.location {
		color: #666;
		font-size: 0.9rem;
		letter-spacing: 1px;
	}

	.footer-contact {
		display: flex;
		justify-content: center;
		gap: 2rem;
		margin-bottom: 2rem;
		flex-wrap: wrap;
	}

	.footer-contact a {
		color: #888;
		text-decoration: none;
		transition: color 0.3s;
	}

	.footer-contact a:hover {
		color: #d4a853;
	}

	.phone {
		font-size: 1.2rem;
		font-weight: 500;
		color: #d4a853 !important;
	}

	.footer-social {
		display: flex;
		justify-content: center;
		gap: 1.5rem;
		margin-bottom: 2rem;
	}

	.footer-social a {
		width: 44px;
		height: 44px;
		display: flex;
		align-items: center;
		justify-content: center;
		border: 1px solid #333;
		border-radius: 50%;
		color: #888;
		transition: all 0.3s ease;
	}

	.footer-social a:hover {
		border-color: #d4a853;
		color: #d4a853;
		transform: translateY(-3px);
	}

	.footer-social svg {
		width: 18px;
		height: 18px;
	}

	.copyright {
		color: #444;
		font-size: 0.85rem;
	}

	@media (max-width: 768px) {
		header {
			padding: 1rem;
		}

		.mobile-toggle {
			display: block;
		}

		.nav-links {
			position: fixed;
			top: 0;
			right: -100%;
			width: 100%;
			height: 100vh;
			background: rgba(10, 10, 10, 0.98);
			flex-direction: column;
			justify-content: center;
			gap: 2rem;
			transition: right 0.4s ease;
		}

		.nav-links.open {
			right: 0;
		}

		.nav-links a {
			font-size: 1.2rem;
		}

		.footer-brand h2 {
			font-size: 1.5rem;
		}
	}
</style>
