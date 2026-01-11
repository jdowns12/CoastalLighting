<script>
	import { onMount } from 'svelte';
	import homepage from '$lib/data/pages/homepage.json';
	import site from '$lib/data/site.json';

	let visible = {
		hero: false,
		features: false,
		about1: false,
		about2: false,
		testimonials: false,
		contact: false
	};

	onMount(() => {
		visible.hero = true;

		const observer = new IntersectionObserver((entries) => {
			entries.forEach(entry => {
				if (entry.isIntersecting) {
					const id = entry.target.id;
					visible[id] = true;
				}
			});
		}, { threshold: 0.2 });

		document.querySelectorAll('section[id]').forEach(section => {
			observer.observe(section);
		});

		return () => observer.disconnect();
	});

	let formData = {
		fullName: '',
		phone: '',
		email: '',
		message: ''
	};

	function handleSubmit(e) {
		e.preventDefault();
		alert('Thank you for your message! We will get back to you soon.');
	}
</script>

<!-- Hero Section -->
<section id="hero" class="hero" class:visible={visible.hero}>
	<div class="hero-bg" style="background-image: url('{homepage.hero.backgroundImage}')"></div>
	<div class="hero-overlay"></div>
	<div class="hero-content">
		<h1>{homepage.hero.title}</h1>
		<p>{homepage.hero.subtitle}</p>
		<div class="hero-buttons">
			{#each homepage.hero.cta as button}
				<a href={button.href} class="btn" class:primary={button.primary}>{button.text}</a>
			{/each}
		</div>
	</div>
</section>

<!-- Features Section -->
<section id="features" class="features" class:visible={visible.features}>
	<div class="section-header">
		<span class="section-subtitle">{homepage.features.sectionSubtitle}</span>
		<h2>{homepage.features.sectionTitle}</h2>
	</div>
	<div class="features-grid">
		{#each homepage.features.items as feature, i}
			<div class="feature-card" style="animation-delay: {i * 0.1}s">
				<span class="feature-number">{feature.number}</span>
				<h3>{feature.title}</h3>
				<p>{feature.description}</p>
			</div>
		{/each}
	</div>
</section>

<!-- About Sections -->
{#each homepage.about as about, i}
	<section id="about{i+1}" class="about" class:visible={visible[`about${i+1}`]} class:reverse={about.imagePosition === 'left'}>
		<div class="about-content">
			<h2>{about.title}</h2>
			<p>{about.description}</p>
		</div>
		<div class="about-image">
			<img src={about.image} alt={about.title} loading="lazy" />
		</div>
	</section>
{/each}

<!-- Testimonials Section -->
<section id="testimonials" class="testimonials" class:visible={visible.testimonials}>
	<div class="section-header">
		<span class="section-subtitle">{homepage.testimonials.sectionSubtitle}</span>
		<h2>{homepage.testimonials.sectionTitle}</h2>
	</div>
	<div class="testimonials-grid">
		{#each homepage.testimonials.items as testimonial, i}
			<div class="testimonial-card" style="animation-delay: {i * 0.15}s">
				<p class="quote">"{testimonial.quote}"</p>
				<div class="author">
					<strong>{testimonial.name}</strong>
					<span>{testimonial.role}</span>
				</div>
			</div>
		{/each}
	</div>
</section>

<!-- Contact Section -->
<section id="contact" class="contact" class:visible={visible.contact}>
	<h2>{homepage.contact.title}<br/><span class="phone">{site.phone}</span></h2>
	<form on:submit={handleSubmit}>
		<div class="form-group">
			<label for="fullName">Full Name <span class="required">*</span></label>
			<input type="text" id="fullName" bind:value={formData.fullName} required />
		</div>
		<div class="form-group">
			<label for="phone">Phone <span class="required">*</span></label>
			<input type="tel" id="phone" bind:value={formData.phone} required />
		</div>
		<div class="form-group">
			<label for="email">Email <span class="required">*</span></label>
			<input type="email" id="email" bind:value={formData.email} required />
		</div>
		<div class="form-group">
			<label for="message">Message <span class="required">*</span></label>
			<textarea id="message" bind:value={formData.message} rows="4" required></textarea>
		</div>
		<p class="terms">By clicking <strong>Submit</strong>, you agree to our <a href="#">Terms of Service</a>.</p>
		<button type="submit" class="submit-btn">Submit</button>
	</form>
</section>

<style>
	/* Hero Section */
	.hero {
		position: relative;
		min-height: 100vh;
		display: flex;
		align-items: center;
		justify-content: center;
		text-align: center;
		color: #fff;
		overflow: hidden;
	}

	.hero-bg {
		position: absolute;
		inset: 0;
		background-size: cover;
		background-position: center;
		transform: scale(1.1);
		transition: transform 8s ease-out;
	}

	.hero.visible .hero-bg {
		transform: scale(1);
	}

	.hero-overlay {
		position: absolute;
		inset: 0;
		background: rgba(0, 0, 0, 0.4);
	}

	.hero-content {
		position: relative;
		z-index: 1;
		max-width: 800px;
		padding: 2rem;
		opacity: 0;
		transform: translateY(30px);
		transition: all 1s ease-out 0.3s;
	}

	.hero.visible .hero-content {
		opacity: 1;
		transform: translateY(0);
	}

	.hero h1 {
		font-size: clamp(2.5rem, 8vw, 5rem);
		font-weight: 800;
		line-height: 1.1;
		margin-bottom: 1.5rem;
	}

	.hero p {
		font-size: clamp(1rem, 2.5vw, 1.25rem);
		margin-bottom: 2rem;
		opacity: 0.95;
	}

	.hero-buttons {
		display: flex;
		gap: 1rem;
		justify-content: center;
		flex-wrap: wrap;
	}

	.btn {
		padding: 0.9rem 2rem;
		border-radius: 6px;
		font-weight: 600;
		text-decoration: none;
		transition: all 0.3s ease;
		font-size: 0.95rem;
	}

	.btn.primary {
		background: #fff;
		color: #1a1a1a;
	}

	.btn.primary:hover {
		background: #f0f0f0;
		transform: translateY(-2px);
	}

	.btn:not(.primary) {
		background: transparent;
		color: #fff;
		border: 2px solid #fff;
	}

	.btn:not(.primary):hover {
		background: rgba(255, 255, 255, 0.1);
	}

	/* Features Section */
	.features {
		padding: 6rem 2rem;
		max-width: 1200px;
		margin: 0 auto;
		opacity: 0;
		transform: translateY(40px);
		transition: all 0.8s ease-out;
	}

	.features.visible {
		opacity: 1;
		transform: translateY(0);
	}

	.section-header {
		text-align: center;
		margin-bottom: 4rem;
	}

	.section-subtitle {
		color: #666;
		font-size: 0.9rem;
		text-transform: uppercase;
		letter-spacing: 1px;
	}

	.section-header h2 {
		font-size: 2.5rem;
		font-weight: 800;
		margin-top: 0.5rem;
	}

	.features-grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
		gap: 2rem;
	}

	.feature-card {
		padding: 1.5rem;
		opacity: 0;
		transform: translateY(20px);
	}

	.features.visible .feature-card {
		animation: fadeInUp 0.6s ease-out forwards;
	}

	.feature-number {
		display: inline-flex;
		align-items: center;
		justify-content: center;
		width: 40px;
		height: 40px;
		background: #1a1a1a;
		color: #fff;
		border-radius: 8px;
		font-weight: 700;
		margin-bottom: 1rem;
	}

	.feature-card h3 {
		font-size: 1.25rem;
		font-weight: 700;
		margin-bottom: 0.75rem;
	}

	.feature-card p {
		color: #666;
		line-height: 1.7;
	}

	/* About Sections */
	.about {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: 4rem;
		padding: 6rem 2rem;
		max-width: 1200px;
		margin: 0 auto;
		align-items: center;
		opacity: 0;
		transform: translateY(40px);
		transition: all 0.8s ease-out;
	}

	.about.visible {
		opacity: 1;
		transform: translateY(0);
	}

	.about.reverse {
		direction: rtl;
	}

	.about.reverse > * {
		direction: ltr;
	}

	.about-content h2 {
		font-size: 2.2rem;
		font-weight: 800;
		margin-bottom: 1.5rem;
		line-height: 1.2;
	}

	.about-content p {
		color: #555;
		font-size: 1.1rem;
		line-height: 1.8;
	}

	.about-image img {
		width: 100%;
		height: 350px;
		object-fit: cover;
		border-radius: 12px;
		box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
	}

	/* Testimonials */
	.testimonials {
		padding: 6rem 2rem;
		background: #fafafa;
		opacity: 0;
		transform: translateY(40px);
		transition: all 0.8s ease-out;
	}

	.testimonials.visible {
		opacity: 1;
		transform: translateY(0);
	}

	.testimonials-grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
		gap: 2rem;
		max-width: 1000px;
		margin: 0 auto;
	}

	.testimonial-card {
		padding: 2rem;
		opacity: 0;
		transform: translateY(20px);
	}

	.testimonials.visible .testimonial-card {
		animation: fadeInUp 0.6s ease-out forwards;
	}

	.quote {
		font-size: 1.1rem;
		font-style: italic;
		line-height: 1.8;
		margin-bottom: 1.5rem;
		color: #333;
	}

	.author strong {
		display: block;
		font-weight: 700;
		margin-bottom: 0.25rem;
	}

	.author span {
		color: #888;
		font-size: 0.9rem;
	}

	/* Contact */
	.contact {
		padding: 6rem 2rem;
		max-width: 600px;
		margin: 0 auto;
		opacity: 0;
		transform: translateY(40px);
		transition: all 0.8s ease-out;
	}

	.contact.visible {
		opacity: 1;
		transform: translateY(0);
	}

	.contact h2 {
		font-size: 2rem;
		font-weight: 800;
		text-align: center;
		margin-bottom: 2.5rem;
	}

	.phone {
		font-size: 2.5rem;
	}

	.form-group {
		margin-bottom: 1.5rem;
	}

	.form-group label {
		display: block;
		font-weight: 600;
		margin-bottom: 0.5rem;
	}

	.required {
		color: #e53935;
	}

	.form-group input,
	.form-group textarea {
		width: 100%;
		padding: 1rem;
		border: 1px solid #ddd;
		border-radius: 8px;
		font-size: 1rem;
		font-family: inherit;
		transition: border-color 0.3s, box-shadow 0.3s;
	}

	.form-group input:focus,
	.form-group textarea:focus {
		outline: none;
		border-color: #1a1a1a;
		box-shadow: 0 0 0 3px rgba(26, 26, 26, 0.1);
	}

	.terms {
		font-size: 0.9rem;
		color: #666;
		margin-bottom: 1.5rem;
	}

	.terms a {
		color: #1a1a1a;
		text-decoration: underline;
	}

	.submit-btn {
		width: 100%;
		padding: 1rem 2rem;
		background: #1a1a1a;
		color: #fff;
		border: none;
		border-radius: 8px;
		font-size: 1rem;
		font-weight: 600;
		cursor: pointer;
		transition: background 0.3s, transform 0.2s;
	}

	.submit-btn:hover {
		background: #333;
		transform: translateY(-2px);
	}

	@keyframes fadeInUp {
		from {
			opacity: 0;
			transform: translateY(20px);
		}
		to {
			opacity: 1;
			transform: translateY(0);
		}
	}

	@media (max-width: 768px) {
		.about {
			grid-template-columns: 1fr;
			gap: 2rem;
		}

		.about.reverse {
			direction: ltr;
		}

		.about-content h2 {
			font-size: 1.8rem;
		}

		.phone {
			font-size: 1.8rem;
		}
	}
</style>
