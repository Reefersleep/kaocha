# master

## Added

- Capture output, this is enabled by default. Only output of failing tests is
  printed. This also introduced a new plugin hook, `wrap-run`, which allows you
  to decorate `run-testables` for doing things like adding bindings.
- Reporters now always get the current testable in their clojure.test reporting
  event.

## Fixed

- The randomize plugin could cause an exception because the
  sort-by-random-number-generator wasn't stable (it violated the contract of a
  comparator). Instead assign each testable a sort key first, then sort by
  those. This does mean seeds from before this change will no longer produce the
  same result.
- When specifying an invalid reporter var, error before trying to load tests.
- Correctly count matcher-combinator mismatch failures when exiting early (Ctrl-C).

# 7b79fad92d (2018-06-16)

## Added

- The profiling plugin can now be configured on the command line and from
  tests.edn, with `--[no-]profiling`, `--profiling-count`,
  `:kaocha.plugin.profiling/profiling?`, `:kaocha.plugin.profiling/count`

## Changed

- `--focus` and `--focus-meta` override config-level `:focus`/`:focus-meta`,
  rather than append. This is more intuitive, when focusing from the command
  line you don't want extra tests to show up.
- Don't run the `post-summary` hook when using the API, this prevents noise from
  plugins in the `--print-test-plan` / `--print-result` output.

## Fixed

- Don't count filtered tests in profiling results.

# 8eeff5b340 (2018-06-13)

## Added

- Testable now has an optional `:kaocha.var/wrap` key, which can contain a seq
  of functions that will be used to "wrap" the actual test function, useful e.g.
  for providing bindings. clojure.test style :each fixtures are also handled
  through this key, so plugins can choose to add wrapping functions at the start
  or the end of this seq to wrap "inside" or "around" the fixtures.

# 9a920204bc (2018-06-13)

## Changed

- Make the test-plan available in `pre-test` and `post-test` plugin hooks, so
  that they have access to top level configuration items.

# 3319ed6f81 (2018-06-13)

## Added

- Added the `kaocha.plugin/defplugin` macro, making plugins look more like a
  deftype.

# 9a6fa32592 (2018-06-02)

## Changed

- The configuration format has changed, you should now start with the `#kaocha
  {}` tagged reader literal in `tests.edn` to provide defaults. If you want more
  control then overwrite `tests.edn` with the output of `--print-config` and
  tweak.