description: "Automated visual tuning: a vision or video model rates rendered variants in a loop. Render several labeled variants into one artifact, ask the model to rate them and suggest better values, render the suggestions, ask it to pick the best, repeat until good; the model is the eye, you run the loop."
Use whenever "looks/feels right" is the success criterion and there's no cheap numeric metric; animation easing/timing, zoom/camera feel, color grade, layout/spacing, design params, render/encoder settings, prompt params. Use the automated counterpart to lookdev when there's no human to sit the loop.
# Visual eval loop: let a vision/video model tune what only an eye can judge
   rubric (define what "good" means; and what "too much"/"too little" look like).
4. **Stop when sufficient**; best rates high and suggestions cluster. Apply the winner.
- **One artifact per round, not one call per variant.** The biggest saver; a 6-variant
  fixed anchors each round; gives the model a reference scale and exposes when its
  (smooth, subtle settle, not bouncy, not sluggish). Don't ask "which do you like" -
  thing being judged is **temporal** (easing, timing, motion smoothness); those are
  spring auto-tune was dropped; the model's pick didn't match the owner's eye.)
- The model's pick is an *opinion*, not ground truth; anchor it, and sanity-check the
- Vision/video models perceive gross differences well, fine ones poorly; keep variant