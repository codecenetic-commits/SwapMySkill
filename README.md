# SwapMySkill Landing Page

A visually stunning, responsive landing page for SwapMySkill - a skill-for-skill exchange marketplace. Built with modern web technologies and focusing on exceptional user experience.

## Features

- **Responsive Design**: Mobile-first approach with breakpoints at 640px, 768px, 1024px, and 1280px
- **Immersive Hero Section**: 3D visualization with Three.js and animated CTAs
- **Smooth Animations**: Framer Motion for UI micro-interactions and GSAP for complex animations
- **Smooth Scrolling**: Lenis for buttery smooth scroll experience
- **Dark/Light Theme**: Toggle between themes with persistent localStorage support
- **Accessible UI**: Semantic HTML, keyboard navigation, and proper ARIA attributes
- **Performance Optimized**: Lazy-loaded 3D scenes and optimized assets
- **User Authentication**: Firebase-powered signup/login with Google OAuth

## Tech Stack

- **Next.js 14** (App Router)
- **Tailwind CSS** (Utility-first styling)
- **Framer Motion** (UI micro-interactions)
- **Lenis** (Smooth scroll)
- **GSAP** (Complex timeline/scroll triggers)
- **Three.js** (Subtle hero/ambient 3D)
- **Shadcn/UI + Radix UI** (Accessible UI primitives)
- **SCSS** (Custom styling tokens)
- **Firebase** (Authentication)

## Getting Started

### Prerequisites

- Node.js 18.x or later
- npm or yarn

### Installation

1. Clone the repository:
   ```bash
   git clone <repository-url>
   ```

2. Navigate to the project directory:
   ```bash
   cd swapmyskill-landing
   ```

3. Install dependencies:
   ```bash
   npm install
   ```

### Development

Run the development server:

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) to view the application.

### Build

Create a production build:

```bash
npm run build
```

### Deployment

Deploy to Vercel or any Next.js compatible hosting platform:

```bash
npm run deploy
```

## Design Tokens

### Colors

- **Primary**: `#3b82f6` (blue-500)
- **Secondary**: `#8b5cf6` (violet-500)
- **Accent**: `#ec4899` (pink-500)
- **Gradients**: 
  - Primary: `linear-gradient(90deg, #3b82f6, #8b5cf6)`
  - Secondary: `linear-gradient(90deg, #8b5cf6, #ec4899)`

### Typography

- **Body Font**: Inter
- **Heading Font**: Plus Jakarta Sans
- **Scale**: xs → 6xl (using Tailwind's default scale)

### Spacing

- xs: 0.25rem
- sm: 0.5rem
- md: 1rem
- lg: 1.5rem
- xl: 2rem
- 2xl: 3rem
- 3xl: 4rem

## Animations & Performance

### Animation Libraries

1. **Framer Motion**: Used for UI micro-interactions, hover effects, and page transitions
2. **GSAP**: Used for complex scroll-triggered animations
3. **Three.js**: Used for the 3D hero visualization

### Performance Considerations

- **Reduced Motion**: All animations respect the `prefers-reduced-motion` media query
- **Lazy Loading**: Three.js scene is dynamically imported and only loaded on client-side
- **GPU Acceleration**: Animations use `transform` and `opacity` for optimal performance
- **Scroll Optimization**: Lenis provides smooth scrolling with minimal performance impact

### Disabling Heavy Animations

To disable heavy animations for performance testing:

1. Enable "Reduce Motion" in your OS settings
2. Or manually set `prefers-reduced-motion: reduce` in browser dev tools

## Accessibility

### Features

- Semantic HTML structure
- Proper heading hierarchy (h1-h6)
- ARIA labels for interactive elements
- Keyboard navigation support
- Focus indicators for interactive elements
- Color contrast >= 4.5:1 for body text
- Screen reader friendly content

### Testing

Tested with:
- Keyboard navigation
- Screen readers
- High contrast mode
- Zoom up to 200%

## Component Structure

```
src/
├── app/
│   ├── layout.tsx
│   ├── page.tsx
│   └── auth/
│       ├── page.tsx
│       └── auth-client.tsx
├── components/
│   ├── Navigation.tsx
│   ├── Hero.tsx
│   ├── ThreeScene.tsx
│   ├── StepFlow.tsx
│   ├── FeaturedExchanges.tsx
│   ├── ExploreSkills.tsx
│   ├── TrustAndSafety.tsx
│   ├── CreatorGrowth.tsx
│   ├── Testimonials.tsx
│   ├── PricingAndFAQ.tsx
│   ├── Footer.tsx
│   ├── ThemeProvider.tsx
│   └── LenisProvider.tsx
├── lib/
│   ├── utils.ts
│   └── firebase.ts
├── styles/
│   └── globals.scss
└── public/
    └── (static assets)
```

## Authentication

The application includes a complete authentication system powered by Firebase Authentication with the following features:

### Routes
- `/auth` - Combined signup/login page with tab switching
- `/dashboard` - User dashboard (protected route)

### Features
- Email/password signup and login
- Google OAuth integration
- Responsive form with validation
- Password visibility toggle
- Loading states and error handling
- Automatic redirect after authentication
- Protected routes for dashboard

### Firebase Configuration
The Firebase configuration is located in `src/lib/firebase.ts` and includes:
- Firebase initialization with provided credentials
- Authentication service setup
- Google provider configuration

## Environment Variables

Create a `.env.local` file in the root directory with the following variables:

```env
NEXT_PUBLIC_CLOUDINARY_API_KEY=your_cloudinary_api_key

```

For Cloudinary setup instructions, see [CLOUDINARY_SETUP.md](CLOUDINARY_SETUP.md).

## Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Open a pull request

## License

This project is licensed under the MIT License.