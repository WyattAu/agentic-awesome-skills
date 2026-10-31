Don't put manual `viewTransitionName` on the root DOM node inside `<ViewTransition>`; React's auto-generated name overrides it.
    setOptimisticSort(nextSort);  // before snapshot; no animation
    setSort(nextSort);            // between snapshots; animates