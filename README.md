# AI-Powered Drupal Blog Publisher

This system helps Drupal developers rapidly generate and publish technical blog articles, including text and illustrative images, directly to a Drupal 11 site. It leverages Google Gemini/OpenAI GPT for text and DALL-E for images, all managed from a local Dockerized environment.

## Quick Start

1.  **Prerequisites:** Ensure you have **Docker Desktop**, **DDEV**, **Python 3.11+**, and valid **Google Gemini** and **OpenAI API keys**.
2.  **Drupal Setup (DDEV):**
    * Set up your Drupal 11 site using DDEV (e.g., `ddev config`, `ddev composer create`, `ddev drush site:install`, `ddev drush en jsonapi rest serialization media`).
    * Create a dedicated API user in Drupal (e.g., `ai_publisher`) with permissions to `create content (Article)`, `create media`, `upload files`, and `access JSON:API`.
3.  **AI Agent Setup:**
    * Clone this repository.
    * Copy `.env.example` to `.env` and fill in your Drupal site URL, API user credentials, and API keys.
        ```bash
        cp .env.example .env
        # Edit .env to add your secrets
        ```
    * Build the AI agent's Docker image:
        ```bash
        docker compose build
        ```

## Usage

The workflow involves generating content, reviewing it, and then publishing it.

### 1. Generate Article & Images

Create a Markdown draft in `drafts/` (e.g., `drafts/my_topic.md`). This can be a mix of English and Russian, outlines, and code placeholders.

Then, run the generation command:

```bash
docker compose run --rm ai-agent python main.py generate --input-file drafts/my_topic.md