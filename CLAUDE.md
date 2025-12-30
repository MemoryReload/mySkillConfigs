# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## High-level Architecture

This repository stores skill configurations for the "Skill Seeker" tool. The main purpose of this tool is to scrape documentation websites and generate Claude skills from the scraped content.

Each configuration is a JSON file located in the `configs/` directory. These files define how to scrape a specific documentation website.

### Configuration File Structure

Each JSON configuration file has the following structure:

- `name`: The name of the skill.
- `description`: A description of the skill.
- `base_url`: The base URL of the documentation website.
- `selectors`: CSS selectors to identify the main content, title, and code blocks on the documentation pages.
- `url_patterns`: Patterns to include or exclude specific URLs during scraping.
- `categories`: A way to categorize the scraped content.
- `rate_limit`: The rate limit for scraping.
- `max_pages`: The maximum number of pages to scrape.

## Common Development Tasks

The primary task in this repository is creating and managing these JSON configuration files.

To run the Skill Seeker tool and scrape the documentation for a specific configuration, you can use the `mcp__skill-seeker__scrape_docs` tool. For example, to scrape the Firebase documentation, you would use the following command:

```
mcp__skill-seeker__scrape_docs --config-path configs/Firebase.json
```

The output of the scraping process will be located in the `output/` directory.
