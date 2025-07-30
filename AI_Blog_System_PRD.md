# Product Requirements Document: AI-Powered Drupal Blog Publishing System

## 1. Introduction

This document outlines the requirements for an AI-powered system designed to streamline the creation and publishing of technical blog articles on a Drupal 11 website. The primary goal is to leverage advanced AI capabilities (specifically Large Language Models and text-to-image generation) to assist a Drupal developer in generating high-quality, technically accurate, and visually engaging blog content, while maintaining full human oversight and approval before publication.

## 2. Goals & Objectives

* **Efficiency:** Reduce the time and effort required to publish technical blog articles.
* **Content Quality:** Generate technically accurate, well-structured, and grammatically correct English articles.
* **Visual Engagement:** Automatically generate relevant and high-quality illustrative/abstract images to accompany articles.
* **Workflow Integration:** Provide a developer-friendly workflow from draft to publication within a containerized local environment.
* **Human Control:** Ensure the developer retains full review and editing capabilities before any content is published live.
* **Open Source / Free Tier Focus:** Utilize free, open-source, or freemium (with existing subscriptions) technologies to minimize operational costs.

## 3. Target Audience

* **Primary User:** The system is built for a technically proficient user comfortable with command-line interfaces, Python scripting, Docker, and Drupal development.
* **Indirect Beneficiary:** Blog readers on the Drupal website, who will benefit from more frequent, high-quality, and visually appealing technical content.

## 4. Scope (Initial Version)

This PRD covers the first iteration of the AI-powered publishing system.

**In Scope:**

* **Local Development Environment:** DDEV for Drupal 11, Docker Compose for AI Agent.
* **Article Generation:**
    * Accepts a Markdown-formatted draft (mix of Russian and English phrases, outlines, technical concepts, placeholder code) as input.
    * Utilizes Google Gemini API or OpenAI GPT API (user's choice via configuration) to expand the draft into a complete, structured, and entirely **English** technical article, including accurate code snippets.
    * Output is a clean Markdown file saved locally.
* **Image Generation:**
    * Generates illustrative/abstract images based on a user-provided prompt (or context derived from the article).
    * Utilizes DALL-E (via OpenAI API).
    * Saves generated images locally as standard image files (e.g., PNG).
* **Review & Editing:**
    * User manually reviews and edits the locally generated Markdown article file and image files using their preferred text/image editor.
    * A clear command-line prompt for "Draft ready to publish" serves as the approval mechanism.
* **Drupal Publishing (JSON:API):**
    * Uploads approved image files to Drupal's Media Library (creating `file` entities then `media--image` entities).
    * Creates a new "Article" content type node in Drupal 11.
    * Populates the article title and body (Markdown content) from the approved local file.
    * Links the uploaded `media--image` entities to the article node via a configured entity reference field.
    * Sets the article status to "Published."
    * Utilizes Basic Authentication for Drupal API communication (over HTTPS, as provided by DDEV).
* **Command-Line Interface:** Simple CLI for `generate` and `publish` commands.
* **Configuration:** API keys and Drupal credentials managed via environment variables (`.env` file).

**Out of Scope (for initial version):**

* Web user interface for the AI agent (e.g., in-browser draft editing, image selection).
* Advanced content types or custom fields beyond basic article structure.
* Automatic SEO optimization suggestions or meta tag generation by AI.
* Multi-lingual article generation beyond English output.
* Automated social media posting upon article publication.
* Complex content scheduling.
* Automated re-generation or updates of existing articles.
* Deployment of the AI agent to a production server (initial focus is local development).

## 5. Functional Requirements

### 5.1. AI Agent Core
* **REQ-AI-001:** The system SHALL accept a Markdown file as input for article generation.
* **REQ-AI-002:** The system SHALL process input drafts containing a mix of Russian and English phrases.
* **REQ-AI-003:** The system SHALL use the Google Gemini API to expand the draft into a full article.
* **REQ-AI-004:** The system SHALL support switching to OpenAI GPT API for article generation via configuration.
* **REQ-AI-005:** The generated article SHALL be entirely in English.
* **REQ-AI-006:** The generated article SHALL include accurate and properly formatted code snippets relevant to the technical content.
* **REQ-AI-007:** The generated article SHALL be structured with headings, paragraphs, and lists as appropriate for a technical blog post.
* **REQ-AI-008:** The system SHALL use the DALL-E API for image generation.
* **REQ-AI-009:** The generated images SHALL be illustrative or abstract in style.
* **REQ-AI-010:** The system SHALL save the generated article as a Markdown file in a designated local `output/` directory.
* **REQ-AI-011:** The system SHALL save generated images (e.g., PNG format) in the designated local `output/` directory.

### 5.2. Workflow & User Interaction
* **REQ-WF-001:** The user SHALL initiate article generation via a command-line interface, specifying the input draft file.
* **REQ-WF-002:** The system SHALL provide a clear message prompting the user to review and edit the generated article and images locally.
* **REQ-WF-003:** The user SHALL manually edit the generated Markdown article and select/rename images as needed.
* **REQ-WF-004:** The user SHALL initiate the publishing process via a separate command-line command, referencing the *approved* local article file.
* **REQ-WF-005:** The system SHALL provide status updates during the generation and publishing processes.

### 5.3. Drupal Integration
* **REQ-DR-001:** The system SHALL authenticate with the Drupal 11 JSON:API using Basic Authentication (username/password of a dedicated API user).
* **REQ-DR-002:** The system SHALL first upload images to Drupal's file system via the `jsonapi/file/file` endpoint.
* **REQ-DR-003:** Upon successful file upload, the system SHALL create a `media--image` entity in Drupal, linking to the uploaded file's UUID.
* **REQ-DR-004:** The system SHALL create a new Drupal "Article" node via the `jsonapi/node/article` endpoint.
* **REQ-DR-005:** The system SHALL populate the "Article" node's `title` field from the first H1 heading of the approved Markdown file.
* **REQ-DR-006:** The system SHALL populate the "Article" node's `body` field with the full Markdown content of the approved file, specifying a "markdown" text format (or converting to HTML if necessary).
* **REQ-DR-007:** The system SHALL link the created `media--image` entities to an entity reference field on the "Article" node (e.g., `field_images`).
* **REQ-DR-008:** The published article in Drupal SHALL have its status set to "Published."

## 6. Non-Functional Requirements

* **Performance:** Article and image generation, and publishing, should complete in a reasonable timeframe (e.g., typically under 5 minutes for generation, under 30 seconds for publishing, depending on API response times and content length).
* **Security:**
    * API keys and credentials SHALL be stored securely as environment variables (not hardcoded).
    * Communication with Drupal SHALL occur over HTTPS.
    * The Drupal API user SHALL have minimum necessary permissions.
* **Maintainability:** The Python codebase SHALL be modular, well-commented, and adhere to Python best practices, facilitating future enhancements.
* **Scalability (Local):** The containerized setup shall allow for easy replication and sharing of the development environment.
* **Reliability:** The system SHALL gracefully handle API errors (e.g., rate limits, invalid responses) and provide informative messages to the user.
* **Usability (for developer):** The command-line interface should be intuitive and provide clear instructions.

## 7. Technical Specifications

* **Drupal Version:** Drupal 11
* **Local Drupal Environment:** DDEV
* **AI Agent Language:** Python 3.x
* **Containerization:** Docker, Docker Compose
* **LLM APIs:** Google Gemini API, OpenAI GPT API
* **Image Generation API:** OpenAI DALL-E API
* **Drupal API:** JSON:API (core module)
* **Python Libraries:** `requests`, `openai`, `google-generativeai`, `python-dotenv`, `markdown` (or similar for Markdown parsing/rendering).
* **Input/Output Formats:** Markdown for text, PNG for images, JSON:API for Drupal communication.

## 8. Future Considerations (Potential Enhancements)

* Integration with other open-source LLMs (e.g., self-hosted Llama variants, Mistral) for local inference (requires significant local GPU).
* Implementation of RAG (Retrieval-Augmented Generation) for highly accurate technical details from a curated knowledge base (e.g., specific documentation repos).
* User-friendly web UI for the AI agent (e.g., built with Flask/FastAPI + a frontend framework).
* Scheduled content generation/publishing.
* AI-powered content summaries or SEO metadata generation.
* Automated social media sharing.
* Support for multiple image placements within an article.
* More sophisticated prompt extraction from drafts for image generation.

## 9. Success Criteria

* The system successfully generates coherent and detailed English technical articles from mixed-language drafts.
* The system consistently generates relevant illustrative/abstract images.
* Articles and images are published to the DDEV Drupal site correctly via API, requiring minimal manual post-publication adjustments.
* The overall workflow significantly reduces the time taken to publish a new technical blog post from initial idea to live content.
* The developer finds the system efficient and enjoyable to use.