# AMCL vs Simple MCL (KISS) Visualizer

Side-by-side comparison of two particle filters localizing the same robot, in a single self-contained HTML file. No build step, no dependencies — open `index.html` in a browser.

## The experiment

One robot, one map, one stream of motion commands and laser scans. Both filters receive identical inputs, so any difference in behavior comes only from the filter itself. Both start from a Gaussian cloud around the true pose (a tracking scenario).

## The two filters

- **Simple MCL (KISS: Keep It Simple, Stupid)**: a fixed number of particles, a Gaussian beam model comparing each particle's predicted scan with the real scan, and systematic resampling when the effective sample size drops below half. No adaptation of any kind.
- **AMCL**: the same predict/update core, plus KLD-sampling (the particle count grows or shrinks with uncertainty) and adaptive random-particle injection driven by fast/slow averages of the sensor likelihood (augmented MCL).

## What to watch

- **Tracking**: both filters follow the robot well, but AMCL needs far fewer particles once confident (watch the particle counts in the stats lines).
- **Kidnap**: press **Kidnap robot** to teleport the robot at least 4 m away. The simple filter usually stays lost — with no way to re-seed hypotheses, its collapsed particle cloud never finds the robot again. AMCL's particle count jumps, injection rises, and the estimate re-converges.

## Controls

- **Auto / Step**: run continuously or one predict+update step at a time (both filters step together).
- **Kidnap robot**: teleport the robot to a random free pose at least 4 m from the current one.
- **Reset**: restart both filters from a fresh Gaussian cloud around the true pose.
- **Simple MCL particles**: fixed particle count for the simple filter (changing it re-initializes that filter only).
- **AMCL max particles**: upper bound for KLD-sampling (100 minimum).
- **Motion noise / Sensor noise**: shared noise levels for the motion model and the laser scan.

## Notes

- The fast/slow likelihood averages use α_fast = 0.2 and α_slow = 0.02, tuned for the demo timescale (the textbook values assume thousands of updates).
- Enable GitHub Pages (Settings → Pages, deploy from main) to run it at https://apocosipadrino.github.io/amcl-kiss-visualizer/
