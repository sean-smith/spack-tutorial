# Spack Tutorial Website

This repo is for the https://spack-tutorial.workshop.aws website.

To edit workshop content:

1. Grab the source

```bash
git clone https://github.com/sean-smith/spack-tutorial.git
cd spack-tutorial/workshop
```

2. Edit & test with hugo

```bash
hugo serve -D
```

3. Create a PR

### Directory Layout

All you need to edit is in `workshop/content/spack` but for the sake of completeness here's what the other directories look like:

```bash
.
├── metadata.yml                      <-- Metadata file with descriptive information about the workshop
├── README.md                         <-- This instructions file
└── workshop                          
    ├── buildspec.yml                 <-- AWS CodeBuild build script for building the workshop website (Note this is being deprecated in favour of automated builds within the workshops.aws platform. You shouldn\'t need to touch this file)
    ├── config.toml                   <-- Hugo configuration file for the workshop website
    └── content                       <-- Markdown files for pages/steps in workshop
    └── static                        <-- Any static assets to be hosted alongside the workshop (ie. images, scripts, documents, etc)
    └── themes                        <-- AWS Style Hugo Theme (Do not edit!)
```
