# Next steps for the SUSE Security product docs

Planned work for the product build. Open questions and known issues are in
[follow-ups.md](follow-ups.md). Remove an item when it is done.

For how the SUSE Security releases and the release config work, see
[How SUSE Security releases work](PRODUCT-consideration.md#how-suse-security-releases-work).

## Installation

Keep all installation content out of the community docs. Use these product
facilities:

- product-only pages and nav entries in `product-specific/` directories
- `{build-type}-specific` partials, with an include point upstream, when a
  shared page must be different in the product build
- the release config

1. **Install SUSE Security for production.** Write a page that installs
   SUSE Security with the umbrella Helm chart. The chart is in development.
   - Put it in the landing component, under
     `docs/kw/version-1.0/modules/en/pages/product-specific/`, and link it
     from the product `landing-nav.adoc`, which this repository owns.
   - If the chart version is different for each release, add it to the
     release config. The extension and the helper check the config strictly,
     so a new key needs a change in both. The extension can then set the
     chart version as a `release-*` attribute on the landing page of each
     release.
2. **Remove the installation instructions of each component.** In the
   product build, the umbrella installation replaces them. The pages come
   from upstream, and the community build still needs them.
   - Add `{build-type}-specific` include points upstream where the install
     steps are, so that the community partial keeps the steps and the product
     partial points to the umbrella installation. The nav entries need the
     same treatment.
   - Decide whether the product build keeps short pages that point to the
     umbrella installation, or leaves the pages out.
3. **Install Admission Controller on its own for Rancher Prime.** Write a
   product-only page for this installation. Put it in the `product-specific/`
   pages of Admission Controller, and add a product nav entry. If the page
   replaces install steps on a shared page, use the include points from
   item 2.

## Publishing

4. **Switch the published site to this repository.** `rancher/product-docs-playbook`
   builds the published site. When this repository goes live:
   1. Remove the demo release 5.9.9 from the release config.
   2. In `product-docs-playbook-remote.yml` and `product-docs-playbook-local.yml`:
      - Replace the `kubewarden-product-docs`, `sbomscanner-product-docs`
        and `runtime-enforcer-product-docs` sources with one source for this
        repository, with the start paths of `remote-product-playbook.yml`.
      - Register `./product-docs-common/extensions/release-url-layout/release-url-layout.js`
        before `vlp.js`, as in `remote-product-playbook.yml`.
   3. Bump the `dsc-style-bundle` and `product-docs-common` submodules of
      `product-docs-playbook` to versions with the release support:
      dsc-style-bundle #90, #91 and #92, and product-docs-common #104.
   4. Build the site with and without the change, and compare. On
      2026-10-28 a build with this repository in place of the old sources was
      clean: no new warnings, and no broken relative links in the SUSE
      Security pages. A build with only the submodule bumps was identical to
      the published site, apart from expected markup.

   See also **Mermaid diagrams in the publishing playbook** and
   **Publishing playbook** in [follow-ups.md](follow-ups.md).

## Decisions

5. **Redirects from the old URLs.** The old per-component URLs, for example
   `/admission-controller/1.38/...` and `/vulnerability-scanner/0.12.0/...`,
   stop working when the published site switches. The decision for now is no
   redirects: SUSE Security is a new product. The extension could add them
   later without a change to the release config.
6. **Archive the old product repositories?** Decide whether to archive
   `kubewarden-product-docs`, `sbomscanner-product-docs` and
   `runtime-enforcer-product-docs` after the published site switches.
