<script>
	import { onMount } from 'svelte';
	import homepage from '\$lib/data/pages/homepage.json';
	import site from '\$lib/data/site.json';

	let visible = {
		hero: false,
		services: false,
		features: false,
		gallery: false,
		testimonials: false,
		contact: false
	};

	let particles = [];
	let activeGalleryIndex = null;

	onMount(() => {
		visible.hero = true;

		// Create floating particles
		for (let i = 0; i < 40; i++) {
			particles.push({
				x: Math.random() * 100,
				y: Math.random() * 100,
				size: Math.random() * 4 + 1,
				speed: Math.random() * 25 + 15,
				delay: Math.random() * 8
			});
		}
		particles = particles;

		const observer = new IntersectionObserver((entries) => {
			entries.forEach(entry => {
				if (entry.isIntersecting) {
					const id = entry.target.id;
					visible[id] = true;
				}
			});
		}, { threshold: 0.1 });

		document.querySelectorAll('section[id]').forEach(section => {
			observer.observe(section);
		});

		return () => observer.disconnect();
	});

	let formData = {
		fullName: '',
		phone: '',
		email: '',
		eventDate: '',
		message: ''
	};

	let formStatus = '';

	function handleSubmit(e) {
		e.preventDefault();
		formStatus = 'sending';
		setTimeout(() => {
			formStatus = 'sent';
			formData = { fullName: '', phone: '', email: '', eventDate: '', message: '' };
		}, 1500);
	}

	function openLightbox(index) {
		activeGalleryIndex = index;
		document.body.style.overflow = 'hidden';
	}

	function closeLightbox() {
		activeGalleryIndex = null;
		document.body.style.overflow = '';
	}

	function nextImage() {
		activeGalleryIndex = (activeGalleryIndex + 1) % homepage.gallery.images.length;
	}

	function prevImage() {
		activeGalleryIndex = (activeGalleryIndex - 1 + homepage.gallery.images.length) % homepage.gallery.images.length;
	}
</script>

<svelte:window on:keydown={(e) => {
	if (activeGalleryIndex !== null) {
		if (e.key === 'Escape') closeLightbox();
		if (e.key === 'ArrowRight') nextImage();
		if (e.key === 'ArrowLeft') prevImage();
	}
}} />

<!-- Hero Section -->
<section id="hero" class="hero" class:visible={visible.hero}>
	<div class="hero-bg" style="background-image: url('{homepage.hero.backgroundImage}')"></div>
	<div class="hero-overlay"></div>
	
	<!-- Floating particles -->
	<div class="particles">
		{#each particles as p}
			<div 
				class="particle" 
				style="left: {p.x}%; top: {p.y}%; width: {p.size}px; height: {p.size}px; animation-duration: {p.speed}s; animation-delay: {p.delay}s;"
			></div>
		{/each}
	</div>
	
	<div class="hero-content">
		<div class="hero-badge">✦ {site.location}</div>
		<h1>{@html homepage.hero.title.replace('\n', '<br/>')}</h1>
		<p>{homepage.hero.subtitle}</p>
		<div class="hero-buttons">
			{#each homepage.hero.cta as button}
				<a href={button.href} class="btn" class:primary={button.primary}>{button.text}</a>
			{/each}
		</div>
	</div>
	
	<div class="scroll-indicator">
		<span>Scroll</span>
		<div class="scroll-line"></div>
	</div>
</section>

<!-- Services Section with Images -->
<section id="services" class="services" class:visible={visible.services}>
	<div class="section-glow"></div>
	<div class="container">
		<div class="section-header center">
			<span class="section-tag">{homepage.services.sectionSubtitle}</span>
			<h2>{homepage.services.sectionTitle}</h2>
		</div>
		<div class="services-grid">
			{#each homepage.services.items as service, i}
				<div class="service-card" style="animation-delay: {i * 0.15}s">
					<div class="service-image">
						<img src={service.image} alt={service.title} loading="lazy" />
						<div class="service-image-overlay"></div>
					</div>
					<div class="service-content">
						<span class="service-icon">{service.icon}</span>
						<h3>{service.title}</h3>
						<p>{service.description}</p>
					</div>
				</div>
			{/each}
		</div>
	</div>
</section>

<!-- Full-width Image Break -->
<section class="image-break">
	<div class="parallax-bg" style="background-image: url('/images/string-lights.jpg')"></div>
	<div class="image-break-overlay"></div>
	<div class="image-break-content">
		<span class="accent-text">Creating Magic</span>
		<h2>Every Light Tells a Story</h2>
	</div>
</section>

<!-- Features Section -->
<section id="features" class="features" class:visible={visible.features}>
	<div class="container">
		<div class="features-layout">
			<div class="features-image">
				<img src="/images/outdoor-event.jpg" alt="Outdoor event lighting" loading="lazy" />
				<div class="features-image-glow"></div>
			</div>
			<div class="features-content">
				<span class="section-tag">{homepage.features.sectionSubtitle}</span>
				<h2>{homepage.features.sectionTitle}</h2>
				<div class="features-list">
					{#each homepage.features.items as feature, i}
						<div class="feature-item" style="animation-delay: {i * 0.2}s">
							<div class="feature-marker">
								<span class="marker-glow"></span>
							</div>
							<div class="feature-text">
								<h3>{feature.title}</h3>
								<p>{feature.description}</p>
							</div>
						</div>
					{/each}
				</div>
			</div>
		</div>
	</div>
</section>

<!-- Gallery Section -->
<section id="gallery" class="gallery" class:visible={visible.gallery}>
	<div class="container">
		<div class="section-header center">
			<span class="section-tag">{homepage.gallery.sectionSubtitle}</span>
			<h2>{homepage.gallery.sectionTitle}</h2>
		</div>
		<div class="gallery-masonry">
			{#each homepage.gallery.images as image, i}
				<button class="gallery-item" style="animation-delay: {i * 0.1}s" on:click={() => openLightbox(i)}>
					<img src={image} alt="Event lighting {i + 1}" loading="lazy" />
					<div class="gallery-overlay">
						<span class="gallery-zoom">+</span>
					</div>
				</button>
			{/each}
		</div>
	</div>
</section>

<!-- Lightbox -->
{#if activeGalleryIndex !== null}
	<div class="lightbox" on:click={closeLightbox}>
		<button class="lightbox-close" on:click={closeLightbox}>&times;</button>
		<button class="lightbox-prev" on:click|stopPropagation={prevImage}>&larr;</button>
		<div class="lightbox-content" on:click|stopPropagation>
			<img src={homepage.gallery.images[activeGalleryIndex]} alt="Gallery image" />
		</div>
		<button class="lightbox-next" on:click|stopPropagation={nextImage}>&rarr;</button>
		<div class="lightbox-counter">{activeGalleryIndex + 1} / {homepage.gallery.images.length}</div>
	</div>
{/if}

<!-- Testimonials Section -->
<section id="testimonials" class="testimonials" class:visible={visible.testimonials}>
	<div class="testimonials-bg"></div>
	<div class="container">
		<div class="section-header center">
			<span class="section-tag">{homepage.testimonials.sectionSubtitle}</span>
			<h2>{homepage.testimonials.sectionTitle}</h2>
		</div>
		<div class="testimonials-grid">
			{#each homepage.testimonials.items as testimonial, i}
				<div class="testimonial-card" style="animation-delay: {i * 0.2}s">
					<div class="quote-mark">"</div>
					<p class="quote">{testimonial.quote}</p>
					<div class="author">
						<div class="author-info">
							<strong>{testimonial.name}</strong>
							<span>{testimonial.role}</span>
						</div>
					</div>
				</div>
			{/each}
		</div>
	</div>
</section>

<!-- Contact Section -->
<section id="contact" class="contact" class:visible={visible.contact}>
	<div class="contact-glow"></div>
	<div class="container">
		<div class="contact-layout">
			<div class="contact-info">
				<h2>{homepage.contact.title}</h2>
				<p>{homepage.contact.subtitle}</p>
				<div class="contact-details">
					<a href="tel:{site.phone}" class="contact-phone">
						<span class="icon">📞</span>
						{site.phone}
					</a>
					<a href="mailto:{site.email}" class="contact-email">
						<span class="icon">✉️</span>
						{site.email}
					</a>
				</div>
			</div>
			<form on:submit={handleSubmit} class="contact-form">
				<div class="form-row">
					<div class="form-group">
						<input type="text" id="fullName" bind:value={formData.fullName} placeholder="Your name" required />
					</div>
					<div class="form-group">
						<input type="tel" id="phone" bind:value={formData.phone} placeholder="Phone number" required />
					</div>
				</div>
				<div class="form-row">
					<div class="form-group">
						<input type="email" id="email" bind:value={formData.email} placeholder="Email address" required />
					</div>
					<div class="form-group">
						<input type="date" id="eventDate" bind:value={formData.eventDate} placeholder="Event date" />
					</div>
				</div>
				<div class="form-group full">
					<textarea id="message" bind:value={formData.message} placeholder="Tell us about your event..." rows="4" required></textarea>
				</div>
				<button type="submit" class="submit-btn" disabled={formStatus === 'sending'}>
					{#if formStatus === 'sending'}
						<span class="spinner"></span> Sending...
					{:else if formStatus === 'sent'}
						✓ Message Sent!
					{:else}
						Send Message
					{/if}
				</button>
			</form>
		</div>
	</div>
</section>

<style>
	/* Container */
	.container {
		max-width: 1200px;
		margin: 0 auto;
		padding: 0 2rem;
	}

	/* Section styling */
	.section-header {
		margin-bottom: 4rem;
	}

	.section-header.center {
		text-align: center;
	}

	.section-tag {
		display: inline-block;
		color: #d4a853;
		font-size: 0.85rem;
		font-weight: 500;
		letter-spacing: 2px;
		text-transform: uppercase;
		margin-bottom: 1rem;
	}

	.section-header h2 {
		font-size: clamp(2rem, 5vw, 3.5rem);
		color: #e8e4de;
		line-height: 1.2;
	}

	/* Hero Section */
	.hero {
		position: relative;
		min-height: 100vh;
		display: flex;
		align-items: center;
		justify-content: center;
		text-align: center;
		overflow: hidden;
	}

	.hero-bg {
		position: absolute;
		inset: 0;
		background-size: cover;
		background-position: center;
		transform: scale(1.1);
		transition: transform 12s ease-out;
		filter: brightness(0.5);
	}

	.hero.visible .hero-bg {
		transform: scale(1);
	}

	.hero-overlay {
		position: absolute;
		inset: 0;
		background: linear-gradient(180deg, rgba(10,10,10,0.4) 0%, rgba(10,10,10,0.85) 100%);
	}

	.particles {
		position: absolute;
		inset: 0;
		overflow: hidden;
		pointer-events: none;
	}

	.particle {
		position: absolute;
		background: radial-gradient(circle, rgba(212, 168, 83, 0.9) 0%, transparent 70%);
		border-radius: 50%;
		animation: float linear infinite;
	}

	@keyframes float {
		0% {
			transform: translateY(100vh) scale(0);
			opacity: 0;
		}
		10% {
			opacity: 1;
		}
		90% {
			opacity: 1;
		}
		100% {
			transform: translateY(-20vh) scale(1);
			opacity: 0;
		}
	}

	.hero-content {
		position: relative;
		z-index: 1;
		max-width: 900px;
		padding: 2rem;
		opacity: 0;
		transform: translateY(40px);
		transition: all 1.2s cubic-bezier(0.4, 0, 0.2, 1) 0.3s;
	}

	.hero.visible .hero-content {
		opacity: 1;
		transform: translateY(0);
	}

	.hero-badge {
		display: inline-block;
		padding: 0.5rem 1.25rem;
		background: rgba(212, 168, 83, 0.15);
		border: 1px solid rgba(212, 168, 83, 0.3);
		border-radius: 50px;
		color: #d4a853;
		font-size: 0.85rem;
		letter-spacing: 1px;
		margin-bottom: 2rem;
	}

	.hero h1 {
		font-size: clamp(3rem, 10vw, 6rem);
		font-weight: 400;
		line-height: 1.05;
		margin-bottom: 1.5rem;
		color: #e8e4de;
	}

	.hero p {
		font-size: clamp(1rem, 2vw, 1.25rem);
		color: #a8a8a8;
		max-width: 600px;
		margin: 0 auto 2.5rem;
		line-height: 1.8;
	}

	.hero-buttons {
		display: flex;
		gap: 1rem;
		justify-content: center;
		flex-wrap: wrap;
	}

	.btn {
		padding: 1rem 2.5rem;
		font-size: 0.95rem;
		font-weight: 500;
		text-decoration: none;
		border-radius: 4px;
		transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
		letter-spacing: 0.5px;
	}

	.btn.primary {
		background: linear-gradient(135deg, #d4a853 0%, #b8923f 100%);
		color: #0a0a0a;
		box-shadow: 0 4px 20px rgba(212, 168, 83, 0.3);
	}

	.btn.primary:hover {
		transform: translateY(-3px);
		box-shadow: 0 8px 30px rgba(212, 168, 83, 0.4);
	}

	.btn:not(.primary) {
		background: transparent;
		color: #e8e4de;
		border: 1px solid rgba(232, 228, 222, 0.3);
	}

	.btn:not(.primary):hover {
		background: rgba(232, 228, 222, 0.1);
		border-color: rgba(232, 228, 222, 0.5);
	}

	.scroll-indicator {
		position: absolute;
		bottom: 3rem;
		left: 50%;
		transform: translateX(-50%);
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 1rem;
		opacity: 0;
		animation: fadeIn 1s ease-out 1.5s forwards;
	}

	.scroll-indicator span {
		color: #666;
		font-size: 0.75rem;
		letter-spacing: 2px;
		text-transform: uppercase;
	}

	.scroll-line {
		width: 1px;
		height: 60px;
		background: linear-gradient(180deg, #d4a853, transparent);
		animation: scrollPulse 2s ease-in-out infinite;
	}

	@keyframes scrollPulse {
		0%, 100% { transform: scaleY(1); opacity: 1; }
		50% { transform: scaleY(0.5); opacity: 0.5; }
	}

	@keyframes fadeIn {
		to { opacity: 1; }
	}

	/* Services Section */
	.services {
		position: relative;
		padding: 8rem 0;
		background: #0a0a0a;
		overflow: hidden;
	}

	.section-glow {
		position: absolute;
		top: 0;
		left: 50%;
		transform: translateX(-50%);
		width: 600px;
		height: 300px;
		background: radial-gradient(ellipse, rgba(212, 168, 83, 0.08) 0%, transparent 70%);
		pointer-events: none;
	}

	.services-grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
		gap: 2rem;
	}

	.service-card {
		position: relative;
		background: linear-gradient(145deg, rgba(30, 30, 30, 0.5), rgba(15, 15, 15, 0.5));
		border: 1px solid rgba(255, 255, 255, 0.05);
		border-radius: 16px;
		overflow: hidden;
		opacity: 0;
		transform: translateY(30px);
		transition: transform 0.4s ease, box-shadow 0.4s ease;
	}

	.services.visible .service-card {
		animation: fadeUp 0.8s cubic-bezier(0.4, 0, 0.2, 1) forwards;
	}

	.service-card:hover {
		transform: translateY(-8px);
		box-shadow: 0 20px 40px rgba(0, 0, 0, 0.4);
	}

	@keyframes fadeUp {
		to {
			opacity: 1;
			transform: translateY(0);
		}
	}

	.service-image {
		position: relative;
		height: 200px;
		overflow: hidden;
	}

	.service-image img {
		width: 100%;
		height: 100%;
		object-fit: cover;
		transition: transform 0.6s ease;
	}

	.service-card:hover .service-image img {
		transform: scale(1.1);
	}

	.service-image-overlay {
		position: absolute;
		inset: 0;
		background: linear-gradient(180deg, transparent 0%, rgba(10, 10, 10, 0.8) 100%);
	}

	.service-content {
		padding: 1.5rem 2rem 2rem;
	}

	.service-icon {
		font-size: 2rem;
		display: block;
		margin-bottom: 1rem;
	}

	.service-card h3 {
		font-size: 1.3rem;
		color: #e8e4de;
		margin-bottom: 0.75rem;
		font-family: 'Inter', sans-serif;
		font-weight: 600;
	}

	.service-card p {
		color: #888;
		font-size: 0.95rem;
		line-height: 1.7;
	}

	/* Image Break */
	.image-break {
		position: relative;
		height: 50vh;
		min-height: 400px;
		display: flex;
		align-items: center;
		justify-content: center;
		text-align: center;
		overflow: hidden;
	}

	.parallax-bg {
		position: absolute;
		inset: -20%;
		background-size: cover;
		background-position: center;
		background-attachment: fixed;
	}

	.image-break-overlay {
		position: absolute;
		inset: 0;
		background: rgba(10, 10, 10, 0.7);
	}

	.image-break-content {
		position: relative;
		z-index: 1;
	}

	.accent-text {
		display: block;
		color: #d4a853;
		font-size: 1rem;
		letter-spacing: 3px;
		text-transform: uppercase;
		margin-bottom: 1rem;
	}

	.image-break-content h2 {
		font-size: clamp(2rem, 5vw, 4rem);
		color: #e8e4de;
	}

	/* Features Section */
	.features {
		padding: 8rem 0;
		background: #0a0a0a;
	}

	.features-layout {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: 4rem;
		align-items: center;
	}

	.features-image {
		position: relative;
		border-radius: 16px;
		overflow: hidden;
	}

	.features-image img {
		width: 100%;
		height: 500px;
		object-fit: cover;
		display: block;
	}

	.features-image-glow {
		position: absolute;
		inset: 0;
		background: linear-gradient(135deg, rgba(212, 168, 83, 0.1) 0%, transparent 50%);
		pointer-events: none;
	}

	.features-content h2 {
		font-size: 2.5rem;
		color: #e8e4de;
		margin: 0.5rem 0 2rem;
	}

	.features-list {
		display: flex;
		flex-direction: column;
		gap: 1.5rem;
	}

	.feature-item {
		display: flex;
		gap: 1.5rem;
		align-items: flex-start;
		opacity: 0;
		transform: translateX(30px);
	}

	.features.visible .feature-item {
		animation: slideIn 0.8s cubic-bezier(0.4, 0, 0.2, 1) forwards;
	}

	@keyframes slideIn {
		to {
			opacity: 1;
			transform: translateX(0);
		}
	}

	.feature-marker {
		position: relative;
		width: 12px;
		height: 12px;
		background: #d4a853;
		border-radius: 50%;
		margin-top: 0.5rem;
		flex-shrink: 0;
	}

	.marker-glow {
		position: absolute;
		inset: -6px;
		background: radial-gradient(circle, rgba(212, 168, 83, 0.4) 0%, transparent 70%);
		border-radius: 50%;
		animation: glowPulse 2s ease-in-out infinite;
	}

	@keyframes glowPulse {
		0%, 100% { transform: scale(1); opacity: 1; }
		50% { transform: scale(1.5); opacity: 0.5; }
	}

	.feature-text h3 {
		font-size: 1.15rem;
		color: #e8e4de;
		margin-bottom: 0.5rem;
		font-family: 'Inter', sans-serif;
		font-weight: 600;
	}

	.feature-text p {
		color: #777;
		line-height: 1.7;
	}

	/* Gallery Section */
	.gallery {
		padding: 8rem 0;
		background: #0f0f0f;
	}

	.gallery-masonry {
		display: grid;
		grid-template-columns: repeat(4, 1fr);
		grid-auto-rows: 200px;
		gap: 1rem;
	}

	.gallery-item {
		position: relative;
		border-radius: 12px;
		overflow: hidden;
		cursor: pointer;
		border: none;
		padding: 0;
		opacity: 0;
		transform: scale(0.95);
	}

	.gallery.visible .gallery-item {
		animation: scaleIn 0.6s cubic-bezier(0.4, 0, 0.2, 1) forwards;
	}

	/* Make some items span 2 rows */
	.gallery-item:nth-child(1),
	.gallery-item:nth-child(4),
	.gallery-item:nth-child(6) {
		grid-row: span 2;
	}

	/* Make some items span 2 columns */
	.gallery-item:nth-child(3),
	.gallery-item:nth-child(7) {
		grid-column: span 2;
	}

	@keyframes scaleIn {
		to {
			opacity: 1;
			transform: scale(1);
		}
	}

	.gallery-item img {
		width: 100%;
		height: 100%;
		object-fit: cover;
		transition: transform 0.6s ease;
	}

	.gallery-item:hover img {
		transform: scale(1.1);
	}

	.gallery-overlay {
		position: absolute;
		inset: 0;
		background: rgba(10, 10, 10, 0.5);
		display: flex;
		align-items: center;
		justify-content: center;
		opacity: 0;
		transition: opacity 0.3s ease;
	}

	.gallery-item:hover .gallery-overlay {
		opacity: 1;
	}

	.gallery-zoom {
		width: 50px;
		height: 50px;
		display: flex;
		align-items: center;
		justify-content: center;
		background: rgba(212, 168, 83, 0.9);
		color: #0a0a0a;
		font-size: 1.5rem;
		font-weight: 300;
		border-radius: 50%;
		transform: scale(0.8);
		transition: transform 0.3s ease;
	}

	.gallery-item:hover .gallery-zoom {
		transform: scale(1);
	}

	/* Lightbox */
	.lightbox {
		position: fixed;
		inset: 0;
		background: rgba(0, 0, 0, 0.95);
		z-index: 1000;
		display: flex;
		align-items: center;
		justify-content: center;
		padding: 2rem;
	}

	.lightbox-content {
		max-width: 90vw;
		max-height: 85vh;
	}

	.lightbox-content img {
		max-width: 100%;
		max-height: 85vh;
		object-fit: contain;
		border-radius: 8px;
	}

	.lightbox-close {
		position: absolute;
		top: 2rem;
		right: 2rem;
		background: none;
		border: none;
		color: #fff;
		font-size: 2.5rem;
		cursor: pointer;
		opacity: 0.7;
		transition: opacity 0.3s;
	}

	.lightbox-close:hover {
		opacity: 1;
	}

	.lightbox-prev,
	.lightbox-next {
		position: absolute;
		top: 50%;
		transform: translateY(-50%);
		background: rgba(255, 255, 255, 0.1);
		border: none;
		color: #fff;
		font-size: 1.5rem;
		padding: 1rem 1.5rem;
		cursor: pointer;
		border-radius: 4px;
		transition: background 0.3s;
	}

	.lightbox-prev { left: 2rem; }
	.lightbox-next { right: 2rem; }

	.lightbox-prev:hover,
	.lightbox-next:hover {
		background: rgba(212, 168, 83, 0.3);
	}

	.lightbox-counter {
		position: absolute;
		bottom: 2rem;
		left: 50%;
		transform: translateX(-50%);
		color: #888;
		font-size: 0.9rem;
	}

	/* Testimonials Section */
	.testimonials {
		position: relative;
		padding: 8rem 0;
		background: #0a0a0a;
		overflow: hidden;
	}

	.testimonials-bg {
		position: absolute;
		inset: 0;
		background: 
			radial-gradient(ellipse at 20% 50%, rgba(212, 168, 83, 0.05) 0%, transparent 50%),
			radial-gradient(ellipse at 80% 50%, rgba(212, 168, 83, 0.05) 0%, transparent 50%);
	}

	.testimonials-grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
		gap: 2rem;
		position: relative;
	}

	.testimonial-card {
		position: relative;
		padding: 2.5rem;
		background: rgba(20, 20, 20, 0.5);
		border: 1px solid rgba(255, 255, 255, 0.05);
		border-radius: 12px;
		opacity: 0;
		transform: translateY(30px);
	}

	.testimonials.visible .testimonial-card {
		animation: fadeUp 0.8s cubic-bezier(0.4, 0, 0.2, 1) forwards;
	}

	.quote-mark {
		font-family: 'Playfair Display', serif;
		font-size: 5rem;
		color: #d4a853;
		opacity: 0.3;
		line-height: 1;
		position: absolute;
		top: 1rem;
		left: 1.5rem;
	}

	.quote {
		font-size: 1.1rem;
		color: #bbb;
		line-height: 1.8;
		margin-bottom: 2rem;
		position: relative;
		z-index: 1;
	}

	.author-info strong {
		display: block;
		color: #e8e4de;
		font-weight: 600;
		margin-bottom: 0.25rem;
	}

	.author-info span {
		color: #666;
		font-size: 0.9rem;
	}

	/* Contact Section */
	.contact {
		position: relative;
		padding: 8rem 0;
		background: #0f0f0f;
		overflow: hidden;
	}

	.contact-glow {
		position: absolute;
		top: 50%;
		left: 0;
		transform: translateY(-50%);
		width: 400px;
		height: 400px;
		background: radial-gradient(circle, rgba(212, 168, 83, 0.1) 0%, transparent 70%);
		pointer-events: none;
	}

	.contact-layout {
		display: grid;
		grid-template-columns: 1fr 1.2fr;
		gap: 4rem;
		align-items: start;
		position: relative;
	}

	.contact-info h2 {
		font-size: 2.5rem;
		color: #e8e4de;
		margin-bottom: 1rem;
	}

	.contact-info > p {
		color: #777;
		margin-bottom: 2rem;
		line-height: 1.7;
	}

	.contact-details {
		display: flex;
		flex-direction: column;
		gap: 1rem;
	}

	.contact-phone,
	.contact-email {
		display: flex;
		align-items: center;
		gap: 1rem;
		color: #999;
		text-decoration: none;
		font-size: 1.1rem;
		transition: color 0.3s;
	}

	.contact-phone {
		color: #d4a853;
		font-size: 1.4rem;
		font-weight: 600;
	}

	.contact-phone:hover,
	.contact-email:hover {
		color: #e8e4de;
	}

	.contact-form {
		background: rgba(20, 20, 20, 0.5);
		border: 1px solid rgba(255, 255, 255, 0.05);
		border-radius: 16px;
		padding: 2.5rem;
	}

	.form-row {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: 1rem;
	}

	.form-group {
		margin-bottom: 1rem;
	}

	.form-group.full {
		grid-column: 1 / -1;
	}

	.form-group input,
	.form-group textarea {
		width: 100%;
		padding: 1rem 1.25rem;
		background: rgba(255, 255, 255, 0.03);
		border: 1px solid rgba(255, 255, 255, 0.1);
		border-radius: 8px;
		color: #e8e4de;
		font-size: 1rem;
		font-family: inherit;
		transition: all 0.3s ease;
	}

	.form-group input::placeholder,
	.form-group textarea::placeholder {
		color: #555;
	}

	.form-group input:focus,
	.form-group textarea:focus {
		outline: none;
		border-color: #d4a853;
		box-shadow: 0 0 0 3px rgba(212, 168, 83, 0.1);
	}

	.submit-btn {
		width: 100%;
		padding: 1.1rem 2rem;
		background: linear-gradient(135deg, #d4a853 0%, #b8923f 100%);
		color: #0a0a0a;
		border: none;
		border-radius: 8px;
		font-size: 1rem;
		font-weight: 600;
		cursor: pointer;
		transition: all 0.4s ease;
		display: flex;
		align-items: center;
		justify-content: center;
		gap: 0.5rem;
	}

	.submit-btn:hover:not(:disabled) {
		transform: translateY(-2px);
		box-shadow: 0 8px 25px rgba(212, 168, 83, 0.3);
	}

	.submit-btn:disabled {
		opacity: 0.7;
		cursor: not-allowed;
	}

	.spinner {
		width: 18px;
		height: 18px;
		border: 2px solid transparent;
		border-top-color: #0a0a0a;
		border-radius: 50%;
		animation: spin 0.8s linear infinite;
	}

	@keyframes spin {
		to { transform: rotate(360deg); }
	}

	/* Responsive */
	@media (max-width: 1024px) {
		.gallery-masonry {
			grid-template-columns: repeat(2, 1fr);
			grid-auto-rows: 180px;
		}

		.gallery-item:nth-child(1),
		.gallery-item:nth-child(4),
		.gallery-item:nth-child(6) {
			grid-row: span 1;
		}

		.gallery-item:nth-child(3),
		.gallery-item:nth-child(7) {
			grid-column: span 1;
		}

		.gallery-item:nth-child(1),
		.gallery-item:nth-child(5) {
			grid-column: span 2;
		}
	}

	@media (max-width: 968px) {
		.features-layout {
			grid-template-columns: 1fr;
			gap: 3rem;
		}

		.features-image {
			order: -1;
		}

		.features-image img {
			height: 350px;
		}

		.contact-layout {
			grid-template-columns: 1fr;
			gap: 3rem;
		}
	}

	@media (max-width: 768px) {
		.hero h1 {
			font-size: clamp(2.5rem, 8vw, 4rem);
		}

		.services, .features, .gallery, .testimonials, .contact {
			padding: 5rem 0;
		}

		.form-row {
			grid-template-columns: 1fr;
		}

		.gallery-masonry {
			grid-template-columns: 1fr;
			grid-auto-rows: 250px;
		}

		.gallery-item:nth-child(n) {
			grid-column: span 1 !important;
			grid-row: span 1 !important;
		}

		.testimonials-grid {
			grid-template-columns: 1fr;
		}

		.contact-info h2 {
			font-size: 2rem;
		}

		.image-break {
			height: 40vh;
			min-height: 300px;
		}

		.parallax-bg {
			background-attachment: scroll;
		}
	}
</style>
