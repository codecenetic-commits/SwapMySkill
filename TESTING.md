# SwapMySkill Landing Page - Test Script

## Overview
This document outlines the steps to test the SwapMySkill landing page to ensure all components and features are working correctly.

## Test Steps

### 1. Homepage
- [ ] Verify the page loads without errors
- [ ] Check that all sections are visible:
  - Navigation bar
  - Hero section with 3D visualization
  - How it works section
  - Featured exchanges
  - Explore skills
  - Trust & safety
  - Creator growth
  - Testimonials
  - Pricing/FAQ
  - Footer

### 2. Navigation
- [ ] Test responsive navigation (mobile menu)
- [ ] Verify smooth scrolling to sections
- [ ] Check hover states and active states

### 3. Hero Section
- [ ] Verify 3D visualization is rendering
- [ ] Test primary and secondary CTAs
- [ ] Check responsive behavior on different screen sizes

### 4. Animations
- [ ] Verify scroll animations are working
- [ ] Check hover animations on interactive elements
- [ ] Test parallax effects
- [ ] Verify reduced motion support

### 5. Theme Toggle
- [ ] Test light/dark mode toggle
- [ ] Verify theme preference is saved in localStorage
- [ ] Check system preference detection

### 6. Responsive Design
- [ ] Test on mobile, tablet, and desktop breakpoints
- [ ] Verify layout adjustments at each breakpoint
- [ ] Check touch interactions on mobile

### 7. Accessibility
- [ ] Test keyboard navigation
- [ ] Verify focus states on interactive elements
- [ ] Check color contrast ratios
- [ ] Test screen reader compatibility

### 8. Design System Page
- [ ] Verify all design system components are displayed
- [ ] Test interactive elements on the page
- [ ] Check responsive behavior

## Performance Testing
- [ ] Verify page load time is under 3 seconds
- [ ] Check bundle size
- [ ] Test animations on low-end devices
- [ ] Verify lazy loading of 3D scenes

## Browser Compatibility
- [ ] Test on latest Chrome
- [ ] Test on latest Firefox
- [ ] Test on latest Safari
- [ ] Test on latest Edge

## Devices to Test
- [ ] iPhone (various sizes)
- [ ] Android phone
- [ ] iPad
- [ ] Desktop monitors (various resolutions)

## Acceptance Criteria
- [ ] All interactive elements are accessible via keyboard
- [ ] All animations respect prefers-reduced-motion
- [ ] Page loads and is visually complete on mobile and desktop
- [ ] Hero has smooth parallax/3D visual and animated CTAs
- [ ] At least three scrolling animations are implemented
- [ ] All major interactive elements have keyboard focus and accessible labels
- [ ] README + local run instructions are included

## Additional Notes
- Test with JavaScript disabled to ensure graceful degradation
- Test with images disabled
- Verify all links are working correctly
- Check for any console errors