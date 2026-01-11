<script>
	import site from '$lib/data/site.json';
</script>

<svelte:head>
	<title>{site.name}</title>
	<meta name="description" content={site.tagline} />
	<link rel="preconnect" href="https://fonts.googleapis.com">
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
	<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
</svelte:head>

<header>
	<nav>
		<a href="/" class="logo">{site.name}</a>
		<div class="nav-links">
			<a href="#features">Why Us?</a>
			<a href="#contact">Contact</a>
		</div>
	</nav>
</header>

<main>
	<slot />
</main>

<footer>
	<div class="footer-content">
		<h2 class="footer-domain">{site.footer.domain}</h2>
		<div class="social-links">
			<a href={site.social.facebook} target="_blank" rel="noopener">Facebook</a>
			<a href={site.social.twitter} target="_blank" rel="noopener">X</a>
			<a href="#contact">Contact</a>
		</div>
		<p class="copyright">&copy; {site.footer.copyright}</p>
		<button class="terms-btn">Terms & Privacy</button>
	</div>
</footer>

<style>
	:global(*) {
		margin: 0;
		padding: 0;
		box-sizing: border-box;
	}

	:global(body) {
		font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
		line-height: 1.6;
		color: #1a1a1a;
		background: #fff;
	}

	:global(html) {
		scroll-behavior: smooth;
	}

	header {
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		z-index: 100;
		padding: 1rem 2rem;
		background: transparent;
		transition: background 0.3s ease;
	}

	nav {
		display: flex;
		justify-content: space-between;
		align-items: center;
		max-width: 1200px;
		margin: 0 auto;
	}

	.logo {
		font-size: 1.1rem;
		font-weight: 600;
		color: #fff;
		text-decoration: none;
	}

	.nav-links {
		display: flex;
		gap: 2rem;
	}

	.nav-links a {
		color: #fff;
		text-decoration: none;
		font-size: 0.95rem;
		font-weight: 500;
		transition: opacity 0.2s;
	}

	.nav-links a:hover {
		opacity: 0.8;
	}

	main {
		min-height: 100vh;
	}

	footer {
		background: #fafafa;
		padding: 4rem 2rem;
		text-align: center;
	}

	.footer-content {
		max-width: 1200px;
		margin: 0 auto;
	}

	.footer-domain {
		font-size: 2.5rem;
		font-weight: 800;
		margin-bottom: 1.5rem;
		color: #1a1a1a;
	}

	.social-links {
		display: flex;
		justify-content: center;
		gap: 2rem;
		margin-bottom: 1.5rem;
	}

	.social-links a {
		color: #666;
		text-decoration: none;
		font-weight: 500;
		transition: color 0.2s;
	}

	.social-links a:hover {
		color: #1a1a1a;
	}

	.copyright {
		color: #888;
		font-size: 0.9rem;
		margin-bottom: 1rem;
	}

	.terms-btn {
		background: none;
		border: none;
		color: #888;
		font-size: 0.9rem;
		cursor: pointer;
		text-decoration: underline;
	}

	@media (max-width: 768px) {
		header {
			padding: 1rem;
		}

		.nav-links {
			gap: 1rem;
		}

		.footer-domain {
			font-size: 1.8rem;
		}
	}
</style>
