In general, the deprecation strategy for changes which only affect builds is
that breaking build changes may be included in any release, but there must be
the ability to opt out of those changes for at least one releases

- A given USD release may both introduce a change behavior AND make
  it the default
- However, that release should provide the ability to keep the old behavior
- Subsequent releases may remove the option / ability to keep old behavior
  entirely


As an example, this would fit the above deprecation strategy:

- OpenUSD v26.03 and all prior versions would set DINO=TREX during build
- The next OpenUSD release, v26.05, makes the default behavior be to set
  DINO=TRICERATOPS
- However, v26.05 also introduces a new `--dino-trex` flag to `build_usd.py`,
  which if provided, sets the cmake variable `DINO_TREX=1`, which in turn makes
  the build use the old DINO=TREX behavior
- The following release, v26.08, removes support for both the `--dino-trex`
  flag and the `DINO_TREX` cmake variable entirely.  v26.08 and all builds
  going forward always set DINO=TRICERATOPS during the build