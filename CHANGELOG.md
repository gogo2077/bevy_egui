# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.35.1] - 9-Jul-2025

### Misc

- Fix the examples in the documentation.

## [0.35.0] - 30-Jun-2025

### Added

- Implement Bevy picking support for meshes ([#394](https://github.com/vladbat00/bevy_egui/pull/394)).
  - You can now implement diegetic UIs with Egui! See the updated [`examples/render_egui_to_image.rs`](./examples/render_egui_to_image.rs) example.

### Changed

- **Breaking change:** Deprecate the option to disable the multi-pass mode ([#387](https://github.com/vladbat00/bevy_egui/pull/387)).
  - The plugin should be initialized with `EguiPlugin::default()` now.
  - The single-pass support is going to be removed in the future. If you still need it for any reason, please let me know in the issues!
- **Breaking change:** Attach `EguiContext` instances to cameras ([#392](https://github.com/vladbat00/bevy_egui/pull/392)).
  - For Egui to render, users now need at least one camera in the world. Egui automatically attaches itself to the first created camera, but more precise control is also possible: for details, see the [examples/side_panel.rs](./examples/side_panel.rs) example.
  - This is also a breaking change for users having multiple windows or accessing the `Window` component together with `EguiContext` (or any other related to Egui) in a single query.  
- **Breaking change:** Refactor `EguiContexts` to support Bevy result systems ([#393](https://github.com/vladbat00/bevy_egui/pull/393)).
  - With this change, systems using `EguiContexts` should be transformed into [Result systems](https://bevy.org/news/bevy-0-16/#unified-ecs-error-handling). 
- Update cursor icons only if changed, make updates disableable ([#388](https://github.com/vladbat00/bevy_egui/pull/388)).

### Fixed

- Fix the panic when closing a window ([#385](https://github.com/vladbat00/bevy_egui/pull/385)).
- Show the virtual keyboard on mobile when text input is focused ([#383](https://github.com/vladbat00/bevy_egui/pull/383) by @arcln).

### Misc

- Remove default features from the `image` dependency ([#380](https://github.com/vladbat00/bevy_egui/pull/380) by @djeedai).
- Add a new example demonstrating split screens: [`examples/split_screen.rs`](./examples/split_screen.rs).

## [0.34.1] - 26-Apr-2025

### Fixed

- Fix panic if `bevy_egui/picking` is enabled, but `bevy/bevy_picking` is not (or `PickingPlugin` is not added).

## [0.34.0] - 25-Apr-2025

### Added

- Implement multi-pass support ([#372](https://github.com/vladbat00/bevy_egui/pull/372)).
  - **Breaking change:** this adds a new `enable_multipass_for_primary_context` field to `EguiPlugin`, see its documentation for the migration guide.  
- Implement absorbing inputs and Egui input run conditions ([#369](https://github.com/vladbat00/bevy_egui/pull/369), [#373](https://github.com/vladbat00/bevy_egui/pull/373)).
- (Disabled until the next Egui release) Integrate AccessKit ([#238](https://github.com/vladbat00/bevy_egui/pull/238) by @ndarilek).

### Changed

- Update Bevy to 0.16 ([#367](https://github.com/vladbat00/bevy_egui/pull/367) by @Friz64).
- Feature-gate `bevy_picking` support behind the `picking` feature ([#363](https://github.com/vladbat00/bevy_egui/pull/363) by @jakkos-net).
- Updated the minimum required version of wasm-binggen to `0.2.93` to match the actual version needed to compile `bevy_egui`.

### Fixed

- Fixed missing `EguiOutput` updates ([#371](https://github.com/vladbat00/bevy_egui/pull/371)).
- Fixed non-latin hotkeys ([#374](https://github.com/vladbat00/bevy_egui/pull/374) by @VinTarZ).
- Fixed multiple windows when a window is spawned later ([#332](https://github.com/vladbat00/bevy_egui/pull/332) by @jabuwu).

### Misc

- Added a side_panel example for 2D ([#253](https://github.com/vladbat00/bevy_egui/pull/253) by @weberja).

## [0.33.0] - 16-Feb-2025

### Changed

- Update Egui to 0.31 ([#357](https://github.com/vladbat00/bevy_egui/pull/357) by @tomara-x).
  - Implement the new `egui::OutputCommand::CopyImage` command support ([#361](https://github.com/vladbat00/bevy_egui/pull/361)).  
- Change `EguiRenderOutput` internals to improve performance, and other optimizations ([#359](https://github.com/vladbat00/bevy_egui/pull/359) by @PPakalns). 

## [0.32.0] - 6-Jan-2025

### Added

- Basic `bevy_picking` support to prevent picking through Egui windows ([#338](https://github.com/vladbat00/bevy_egui/pull/338), [#331](https://github.com/vladbat00/bevy_egui/pull/331) by @globin and @aevyrie).
- Helpers for converting between Egui and Bevy types ([#345](https://github.com/vladbat00/bevy_egui/pull/345), [488ac6a](https://github.com/vladbat00/bevy_egui/commit/488ac6a9d8fc3e1be98b494aed83751415beb845)).

### Changed

- Update Egui to 0.30 ([#340](https://github.com/vladbat00/bevy_egui/pull/340) by @PPakalns).
- Reuse IDs of removed textures ([#344](https://github.com/vladbat00/bevy_egui/pull/344)).
- Input handling refactor ([#345](https://github.com/vladbat00/bevy_egui/pull/345)).
  - This brings us closer to diegetic (world space) UI support, the `HoveredNonWindowEguiContext`
    and `FocusedNonWindowEguiContext` resources can now be used to redirect input
    events to other contexts.
  - Input handling was split into separate systems for each event type, which are now also disableable ([#346](https://github.com/vladbat00/bevy_egui/pull/346)), 
    see the `EguiGlobalSettings` resource.
  - The `EguiInputEvent` event type was added, which wraps all the events that are sent to Egui.
    It can also be used for custom input events logic, in case you want to mock inputs or handle inputs yourself. 
  - The `EguiSettings` component was renamed to `EguiContextSettings`.
  - `EguiSet` was split into the `EguiPreUpdateSet` and `EguiPostUpdateSet` sets.
    - `EguiInputSet` was also added (as `EguiPreUpdateSet::ProcessInput` subsets).
- Egui contexts are now immediately initialised for entities with a `EguiRenderToImage` component
  (as opposed to being initialised with a delay by a separate system),
  thanks to Bevy [required components](https://docs.rs/bevy/0.15.1/bevy/prelude/trait.Component.html#required-components).

### Fixed

- Fix clipboard error handling ([#347](https://github.com/vladbat00/bevy_egui/pull/347)).

## [0.31.1] - 30-Nov-2024

### Fixed

- Fix docs.rs build.

## [0.31.0] - 30-Nov-2024

### Changed

- Update to Bevy 0.15 ([#309](https://github.com/vladbat00/bevy_egui/pull/309) by @Vrixyz).

### Fixed

- Don't add `EguiContext` to every entity when the `render` feature is disabled ([#321](https://github.com/vladbat00/bevy_egui/pull/321) by @DGriffin91).

## [0.30.1] - 10-Nov-2024

### Changed

- Depend on bevy subcrates for compile time improvement ([#319](https://github.com/vladbat00/bevy_egui/pull/319) by @aevyrie).

## [0.30.0] - 4-Oct-2024

### Added

- `prepare_render` step support for `EguiBevyPaintCallbackImpl` ([#306](https://github.com/vladbat00/bevy_egui/pull/306) by @PPakalns).
- Mobile virtual keyboard support in web ([#279](https://github.com/vladbat00/bevy_egui/pull/279) by @v-kat).
  - Requires `Window::prevent_default_event_handling` being set to `false`.
- IME support (#[204](https://github.com/vladbat00/bevy_egui/pull/204) by @EReeves).

### Changed

- Update Egui to 0.29 ([#313](https://github.com/vladbat00/bevy_egui/pull/313) by @PPakalns).

### Additional notes on breaking changes

- `EguiSettings` is now a component.
- `EguiSet::BeginFrame` has been renamed to `EguiSet::BeginPass`.

## [0.29.0] - 18-Aug-2024

### Added

- Initial worldspace UI support ([#304](https://github.com/vladbat00/bevy_egui/pull/304) by @TheButlah, @Schmarni-Dev).
- Paint callback support ([#303](https://github.com/vladbat00/bevy_egui/pull/303) by @PPakalns).

### Changed

- Adapt to `web-sys` clipboard api change ([#301](https://github.com/vladbat00/bevy_egui/pull/301) by @no-materials).

### Fixed

- Clear modifier state when focus is lost ([#298](https://github.com/vladbat00/bevy_egui/pull/298) by @SludgePhD).
- Fix redraws ([#293](https://github.com/vladbat00/bevy_egui/pull/293)).

## [0.28.0] - 6-Jul-2024

### Changed

- Update Bevy to 0.14 ([#284](https://github.com/vladbat00/bevy_egui/pull/284) by @Friz64).
- Update Egui to 0.28 ([#290](https://github.com/vladbat00/bevy_egui/pull/290) by @Swoorup).
- Update webbrowser to 1.0.1

## [0.27.1] - 2-Jun-2024

### Changed

- Request Redraw only if really needed ([#278](https://github.com/vladbat00/bevy_egui/pull/278) by @Maximetinu).
- Fix light in the `render_to_image_wideget` example ([#282](https://github.com/vladbat00/bevy_egui/pull/282) by @rlidwka).

## [0.27.0] - 18-Apr-2024

### Added

- Fallible variants of primary window getters for `EguiContexts`.

### Changed

- Update Egui to 0.27 ([#271](https://github.com/vladbat00/bevy_egui/pull/271) by @jakobhellermann).
- Improve compilation errors when missing `web_sys_unstable_apis` ([#270](https://github.com/vladbat00/bevy_egui/pull/270) by @Vrixyz).

### Fixed

- Rework reading window ids for events (fixes edge-cases with ignoring events, [#273](https://github.com/vladbat00/bevy_egui/pull/273)).

### Removed

- Unused `RenderGraphConfig`.

## [0.26.0] - 18-Mar-2024

### Added

- Add web clipboard support ([#267](https://github.com/vladbat00/bevy_egui/pull/267), [#178](https://github.com/vladbat00/bevy_egui/pull/178) by @Vrixyz).

### Fixed

- Respect `egui::TextureOptions` for managed textures ([#264](https://github.com/vladbat00/bevy_egui/pull/264) by @TheRawMeatball).
- Fix keybind modifiers ([#265](https://github.com/vladbat00/bevy_egui/pull/265) by @eero-lehtinen).

## [0.25.0] - 19-Feb-2024

### Added

- Add `render` feature which can be disabled for applications with a custom renderer ([#240](https://github.com/vladbat00/bevy_egui/pull/240) by @BeastLe9enD).

### Changed

- Update Bevy to 0.13 ([#236](https://github.com/vladbat00/bevy_egui/pull/236) by @eri).
- Update Egui to 0.26.

### Fixed

- Retrieve user agent for better platform detection on WASM ([#256](https://github.com/vladbat00/bevy_egui/pull/256) by @Vrixyz).
- Remove unused `once_cell` dev-dependency ([#258](https://github.com/vladbat00/bevy_egui/pull/258) by @frewsxcv).
- Make fields inside `WindowSize` pub ([#251](https://github.com/vladbat00/bevy_egui/pull/251) by @BeastLe9enD).
- Fix requested repaints not causing Bevy to redraw ([#240](https://github.com/vladbat00/bevy_egui/pull/240) by @andriyDev).
- Fix build on Android with default features ([#241](https://github.com/vladbat00/bevy_egui/pull/241) by @Hellzbellz123).

## [0.24.0] - 11-Dec-2023

### Changed

- Update Egui to 0.24 ([#234](https://github.com/vladbat00/bevy_egui/pull/234) by @naomijub, @frewsxcv).

### Fixed

- Handle giving time input to egui correctly ([#226](https://github.com/vladbat00/bevy_egui/pull/226) by @TheRawMeatball).

## [0.23.0] - 5-Nov-2023

### Changed

- Update Bevy to 0.12 ([#221](https://github.com/vladbat00/bevy_egui/pull/221) by @raffaeleragni).

### Fixed

- Fix color attachments in WASM (WebGPU) ([#220](https://github.com/vladbat00/bevy_egui/pull/220) by @seabassjh, @frewsxcv).

## [0.22.0] - 7-Oct-2023

### Added

- Add `#[derive(Reflect)]` ([#195](https://github.com/vladbat00/bevy_egui/pull/195) by @SludgePhD).

### Changed

- Update Egui to 0.23 ([#217](https://github.com/vladbat00/bevy_egui/pull/217) by @zicklag).
- Refactor components and resources extraction ([#210](https://github.com/vladbat00/bevy_egui/pull/210), [#211](https://github.com/vladbat00/bevy_egui/pull/211) by @TheButlah).

## [0.21.0] - 10-Jul-2023

### Added

- Add touch events support ([#180](https://github.com/vladbat00/bevy_egui/pull/180) by @oscrim).

### Changed

- Update Bevy to 0.11 ([#188](https://github.com/vladbat00/bevy_egui/pull/188) by @Vrixyz).
- Update Egui to 0.22 ([#184](https://github.com/vladbat00/bevy_egui/pull/184)).
- Move sampler descriptor into `EguiSettings` ([#179](https://github.com/vladbat00/bevy_egui/pull/179) by @GlummixX).
- Update GitHub Actions CI ([#183](https://github.com/vladbat00/bevy_egui/pull/183) by @striezel).

## [0.20.3] - 21-Apr-2023

### Fixed

- Accept NumpadEnter as Enter ([#171](https://github.com/vladbat00/bevy_egui/pull/171) by @dimvoly).

## [0.20.2] - 27-Mar-2023

### Changed

- Move `bevy_core_pipeline` to dev-dependencies ([#166](https://github.com/vladbat00/bevy_egui/pull/166) by @jakobhellermann).

### Fixed

- Fix incorrect bounds check for set_scissor_rect ([#167](https://github.com/vladbat00/bevy_egui/pull/167) by @Gorialis).
- Fix panic messages for uninitialised contexts.

## [0.20.1] - 12-Mar-2023

### Fixed

- Fix recreation of `EguiContext` on startup ([#162](https://github.com/vladbat00/bevy_egui/pull/162) by @encounter).
- Set image sampler address modes to `ClampToEdge` ([#158](https://github.com/vladbat00/bevy_egui/pull/158) by @galop1n).

## [0.20.0] - 8-Mar-2023

### Added

- Add `altgr` support for Windows ([#149](https://github.com/vladbat00/bevy_egui/pull/149) by @Vrixyz).
- Add `serde` feature ([#154](https://github.com/vladbat00/bevy_egui/pull/154) by @AlanRace).

### Changed

- Update Bevy to 0.10 ([#159](https://github.com/vladbat00/bevy_egui/pull/159), thanks to @DGriffin91).
- Update Egui to 0.21 ([#152](https://github.com/vladbat00/bevy_egui/pull/152) by @paul-hansen).
- Implement better multi-window support ([#147](https://github.com/vladbat00/bevy_egui/pull/147) by @TheRawMeatball).

### Fixed

- Pass raw Bevy time to Egui to fix UI animations ([#155](https://github.com/vladbat00/bevy_egui/pull/155) by @jakobhellermann).

## [0.19.0] - 15-Jan-2023

### Changed

- Update the `arboard` dependency ([#142](https://github.com/vladbat00/bevy_egui/pull/142) by @jakobhellermann).

### Fixed

- Fix panics due to missing swapchain textures ([#141](https://github.com/vladbat00/bevy_egui/pull/141) by @connerebbinghaus).

## [0.18.0] - 11-Dec-2022

### Changed

- Update Egui to 0.20 ([#139](https://github.com/vladbat00/bevy_egui/pull/139) by @no-materials).

## [0.17.1] - 14-Nov-2022

### Fixed

- Fix clearing event readers (missed events warnings).

## [0.17.0] - 13-Nov-2022

### Changed

- Update to Bevy 0.9 ([#127](https://github.com/vladbat00/bevy_egui/pull/127), [#133](https://github.com/vladbat00/bevy_egui/pull/133), thanks to @terhechte and @jakobhellermann).

### Fixed

- Fix window resizing on Windows ([#128](https://github.com/vladbat00/bevy_egui/pull/128) by @chronicl). 

## [0.16.1] - 18-Sep-2022

### Fixed

- Fix releasing buttons outside a window ([#123](https://github.com/vladbat00/bevy_egui/pull/123), thanks to @TheRawMeatball for flagging the issue in [#121](https://github.com/vladbat00/bevy_egui/pull/121)).

## [0.16.0] - 24-Aug-2022

### Changed

- Update Egui to 0.19.

## [0.15.1] - 13-Aug-2022

### Fixed

- Store image handles instead of ids to persist strong handles.

## [0.15.0] - 30-Jul-2022

### Added

- Add a feature that can be disabled to replace default Egui fonts ([#110](https://github.com/vladbat00/bevy_egui/pull/110) by @iTitus).

### Changed
 
- Update Bevy to 0.8 ([#111](https://github.com/vladbat00/bevy_egui/pull/111) by @DGriffin91).

## [0.14.0] - 1-May-2022

### Added

- Add new_tab support for open_url ([#96](https://github.com/vladbat00/bevy_egui/pull/96) by @Azorlogh).
  - `EguiSettings` has also got the `default_open_url_target` parameter to make the default behaviour on left mouse click configurable.
- Update Egui to 0.18 ([#99](https://github.com/vladbat00/bevy_egui/pull/99)).

### Changed

- The `multi_threaded` feature was renamed to `immutable_ctx`.

### Fixed

- Improve wgsl readability and introduce minor optimisations ([#95](https://github.com/vladbat00/bevy_egui/pull/95) by @lain-dono).
- Remove duplicate EguiPipeline resource initialization ([#98](https://github.com/vladbat00/bevy_egui/pull/98) by @lain-dono).
- Fix color blending for user textures ([#100](https://github.com/vladbat00/bevy_egui/pull/100)).

## [0.13.0] - 16-Apr-2022

### Changed

- Update Bevy to 0.7 ([#79](https://github.com/vladbat00/bevy_egui/pull/79) by @aevyrie and @forbjok).
- Return egui::TextureId on removal ([#81](https://github.com/vladbat00/bevy_egui/pull/81) by @Shatur).
- Add `must_use` attributes to methods ([#82](https://github.com/vladbat00/bevy_egui/pull/82)).

### Fixed

- Remove unnecessary image clone allocation ([#84](https://github.com/vladbat00/bevy_egui/pull/84) by @frewsxcv).
- Avoid allocations by utilizing `HashMap::iter_mut` ([#83](https://github.com/vladbat00/bevy_egui/pull/83) by @frewsxcv).
- Remove unnecessary swap texture clone ([#85](https://github.com/vladbat00/bevy_egui/pull/85) by @frewsxcv).

## [0.12.1] - 13-Mar-2022

### Added

- Add a function to get image id ([#80](https://github.com/vladbat00/bevy_egui/pull/80) by @Shatur).

## [0.12.0] - 12-Mar-2022

### Added

- Add side panel example ([#73](https://github.com/vladbat00/bevy_egui/pull/73)).

### Changed

- Update Egui to 0.17 ([#78](https://github.com/vladbat00/bevy_egui/pull/78) by @emilk).

### Fixed

- User texture ids are now tracked internally ([#71](https://github.com/vladbat00/bevy_egui/pull/71)).
  - Instead of using `set_egui_texture`, you can now use `add_image` which returns a texture id itself
    (see the updated [ui](https://github.com/vladbat00/bevy_egui/blob/c611671603a70e5956ba06f77bb94851c7ced659/examples/ui.rs) example).
- Switch to `arboard` for managing clipboard ([#72](https://github.com/vladbat00/bevy_egui/pull/72)).

## [0.11.1] - 4-Feb-2022

### Added

- Add `ctx_for_windows_mut` and `try_ctx_for_windows_mut` for accessing multiple contexts without the `multi_threaded` feature.

## [0.11.0] - 4-Feb-2022

### Changed

- Introduce mutable getters for EguiContext, feature gate immutable ones ([#64](https://github.com/vladbat00/bevy_egui/pull/63)).
  - If you used `bevy_egui` without the `multi_threaded` feature, you'll need to change every `ctx` call to `ctx_mut`.

## [0.10.3] - 29-Jan-2022

### Added

- Feature `multi_threaded`, to avoid using `egui/multi_threaded` ([#63](https://github.com/vladbat00/bevy_egui/pull/63) by @ndarilek).

### Fixed

- WGPU crash on minimizing a window ([#62](https://github.com/vladbat00/bevy_egui/pull/62) by @aevyrie).

## [0.10.2] - 23-Jan-2022

### Added

- Horizontal scroll support (Shift + Mouse Wheel).
- Zoom support (Ctrl/Cmd + Mouse Wheel).

### Fixed

- Change points delta from 24 to 50 for `MouseScrollUnit::Line` event.
- Fix handling of mouse button events for Safari (inputs are no longer ignored).
- Scroll is no longer applied to every Bevy window.

## [0.10.1] - 16-Jan-2022

### Added

- Headless mode support ([#51](https://github.com/vladbat00/bevy_egui/pull/51) by @Shatur).

### Fixed

- Egui pass now runs after `bevy_ui` ([#53](https://github.com/vladbat00/bevy_egui/pull/53) by @jakobhellermann).

## [0.10.0] - 8-Jan-2022

### Changed

- Update Bevy to 0.6 ([#25](https://github.com/vladbat00/bevy_egui/pull/25) by @jakobhellermann).

## [0.9.0] - 1-Jan-2022

### Changed

- Update Egui to 0.16 ([#49](https://github.com/vladbat00/bevy_egui/pull/49) by @Meshiest).

## [0.8.0] - 27-Nov-2021

### Changed

- Update Egui to 0.15.0 ([#45](https://github.com/vladbat00/bevy_egui/pull/45)).

## [0.7.1] - 06-Oct-2021

### Added

- Add `EguiStartupSystem` system labels.

### Fixed

- Initialize egui contexts during startup (fixes [#41](https://github.com/vladbat00/bevy_egui/issues/41)).

## [0.7.0] - 05-Sep-2021

### Changed

- Update Egui to 0.14.0 ([#38](https://github.com/vladbat00/bevy_egui/pull/38)).

## [0.6.2] - 15-Aug-2021

### Fixed

- Fix receiving input when holding a button ([#37](https://github.com/vladbat00/bevy_egui/pull/37)).

## [0.6.1] - 20-Jul-2021

### Fixed

- Fix more edge-cases related to invalid scissors.

## [0.6.0] - 29-Jun-2021

### Changed

- Update Egui to 0.13.0.

## [0.5.0] - 22-May-2021

### Changed

- Update Egui to 0.12.0.

## [0.4.2] - 03-May-2021

### Added

- Better error message for a missing Egui context ([#24](https://github.com/vladbat00/bevy_egui/pull/24) by @jakobhellermann).
- Add `try_ctx_for_window` function ([#20](https://github.com/vladbat00/bevy_egui/pull/20) by @jakobhellermann).

## [0.4.1] - 24-Apr-2021

### Fixed

- Fix crashes related to invalid scissor or window size ([#18](https://github.com/vladbat00/bevy_egui/pull/18)).

## [0.4.0] - 10-Apr-2021

Huge thanks to @jakobhellermann and @Weasy666 for contributing to this release!

### Added

- Implement multiple windows support ([#14](https://github.com/vladbat00/bevy_egui/pull/14) by @jakobhellermann).

### Changed

- Update Egui to 0.11.0 ([#12](https://github.com/vladbat00/bevy_egui/pull/12) by @Weasy666 and @jakobhellermann).

## [0.3.0] - 02-Mar-2021

### Changed

- Update Egui to 0.10.0.

## [0.2.0] - 08-Feb-2021

### Changed

- Update Egui to 0.9.0.

## [0.1.3] - 20-Jan-2021

### Fixed

- Fix copying textures to take alignment into account.
- Disable a documentation test.

## [0.1.2] - 18-Jan-2021

### Fixed

- Disable default features for docs.rs to fix the build.

## [0.1.1] - 18-Jan-2021

### Fixed

- Fix compilation errors when no features are set.
