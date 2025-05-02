# Project FETA - Flow-based Encrypted Traffic Analysis

This repository contains the website for Project FETA, which focuses on flow-based encrypted traffic analysis.

## About Project FETA

Project FETA is dedicated to the study and implementation of flow-based encrypted traffic analysis. The project is a collaboration between [FIT CTU in Prague](https://www.fit.cvut.cz/), [CESNET](https://cesnet.cz/), and [FIT BUT](https://www.fit.vutbr.cz/).

## Website Structure

The website is built using Jekyll with the Just-the-Docs theme. Key content areas include:

- Classification methods
- Datasets information
- Dataset creation processes
- Hardware acceleration
- QRadar integration

## Contributing to the Website

There are two ways to set up the development environment:

### Option 1: Standard Jekyll Setup

1. **Prerequisites**
   - Ruby

2. **Clone the repository**
   ```bash
   git clone git@github.com:CESNET/project-feta-web.git
   cd project-feta-web
   ```

3. **Start the local development server**
   ```bash
   bundle exec jekyll serve
   ```
   This will start a local server where you can preview your changes.

### Option 2: Using Nix Flakes with direnv

This is an alternative approach that ensures a consistent environment across all contributors.

1. **Prerequisites**
   - Install [Nix](https://nixos.org/download.html)
   - Install [direnv](https://direnv.net/docs/installation.html)
   - Configure direnv in your shell

2. **Set up the development environment**
   ```bash
   # First time only: Allow direnv to load the environment
   direnv allow
   
   # The environment will automatically load when you enter the directory
   ```

3. **Start the local development server**
   ```bash
   cd web
   bundle exec jekyll serve
   ```

### Adding or Modifying Content

The website content is organized in the `web/source/` directory by topic:

- `classification/` - For classification-related content
- `datasets/` - For dataset information
- `datasets_creation/` - For documentation on creating datasets
- `hardware/` - For hardware-related topics
- `qradar/` - For QRadar integration documentation

#### Adding a New Page

1. Create a new Markdown file in the appropriate subdirectory of `web/source/`
2. Add the front matter at the top of your Markdown file:
   ```yaml
   ---
   layout: default
   title: Your Page Title
   nav_order: 1  # Adjust based on where you want it in the navigation
   parent: Parent Page Title  # If this is a child page
   ---
   ```
3. Add your content using Markdown syntax

#### Working with Images and Other Assets

Place static assets in the `web/assets/` directory in an appropriate subdirectory.

### Testing Your Changes

```bash
bundle exec jekyll build
```

### Using Callouts

The theme supports various callout styles that you can use in your Markdown:

```markdown
{: .note }
This is a note callout.

{: .warning }
This is a warning callout.

{: .important }
This is an important callout.
```

### Submitting Changes

1. Create a new branch for your contribution
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. Commit your changes. Use conventional commits.


3. Push changes and create a pull request
   ```bash
   git push origin feature/your-feature-name
   ```

4. Submit a pull request through the repository's web interface

## License

See the [LICENSE](LICENSE) file for details.