# Desktop updater owner-transfer rehearsal

Install **Build A** on a disposable machine or VM to repeat the upgrade. Open Settings, check for updates, and choose **Update now**. The update should install **Build B** and restart Desktop. These builds use Unsloth’s normal app identifier and install paths, plus a dedicated test update channel and signing key.

| Release | Source | Downloads |
| --- | --- | --- |
| A · `0.1.9100-beta` | Upstream `f1795531ac` with the legacy owner check and the rehearsal endpoint | [Build A](https://github.com/unsloth-test-rename/desktop-updater-transfer-lab/releases/tag/v0.1.9100-beta) |
| B · `0.1.9101-beta` | The same upstream commit plus PR #18593 at `e461391`, with the rehearsal owners substituted | [Build B](https://github.com/unsloth-test-rename/desktop-updater-transfer-lab/releases/tag/v0.1.9101-beta) |

A was published under `wasimysaid/desktop-updater-transfer-lab`. The repository was then transferred to `unsloth-test-rename`, retaining repository ID `1465364046`. B was built and published after the transfer. Both clients discover updates at the original URL, and B’s manifest still pins its versioned asset URLs to the old owner. The signing jobs ran in the personal fork. [Build A run](https://github.com/wasimysaid/unsloth/actions/runs/44730286411) · [Build B run](https://github.com/wasimysaid/unsloth/actions/runs/44732733516)

## Observed results

- On Ubuntu 24.04 x64, the `.deb` installed before the transfer and an AppImage installed through the old link both offered B in Settings. Clicking **Update now** updated each installation. After relaunch, the running executables matched the published B binaries, and both chat-only backends reported healthy.
- An extracted Linux package running A selected manual-update mode. After the transfer, it accepted B’s manifest and returned B’s version. The old release-page link resolved to the new owner.
- All six distinct updater bundles for each version matched GitHub’s SHA-256 digests and verified with the test public key. B’s original-owner manifest URL followed three redirects to HTTP 200. Its original-owner asset URLs followed two.
- The pointer repair workflow restored the complete pre-transfer A release after an empty release was marked Latest. The release workflow subsequently promoted B, and the post-transfer pointer workflow validated it. [Restore A](https://github.com/unsloth-test-rename/desktop-updater-transfer-lab/actions/runs/44732809780) · [Publish B](https://github.com/unsloth-test-rename/desktop-updater-transfer-lab/actions/runs/44734834739) · [Validate B](https://github.com/unsloth-test-rename/desktop-updater-transfer-lab/actions/runs/44735150658)
- On Windows x64, A completed chat-only setup, offered B in Settings, and relaunched as B after **Update now**. The running WebView2 process reported the new app version. [Windows Settings upgrade](https://github.com/wasimysaid/unsloth/actions/runs/44737099066)
- On Windows x64 and ARM64, both installers passed Authenticode validation. The NSIS update handoff installed B over A and launched B on both architectures. [Windows installer handoff](https://github.com/unsloth-test-rename/desktop-updater-transfer-lab/actions/runs/44735777766)
- On Apple Silicon, the two disk images passed stapled-ticket verification. Gatekeeper accepted each installed app, and both launched. [macOS check](https://github.com/unsloth-test-rename/desktop-updater-transfer-lab/actions/runs/44735146151)
