Spline scenes are WebGL; they run on the GPU. A poorly optimized scene will tank your PageSpeed score, lag on mid-range devices, and drain mobile batteries. Treat them like video files, not images.
## Before You Even Integrate: Check Scene Size
- Over 20MB = do not embed as live 3D; export as video instead
5. Use fewer than 3 lights; prefer Matcap materials for reflective effects (fakes reflections without GPU cost)
  background: #0a0a0a; /* your site's bg color; shows instantly */
### Option A: Skip entirely on mobile (recommended for hero backgrounds)
  // Don't load Spline; show static background instead
### Option B: Hardware concurrency check
### Option C: Export as video for mobile