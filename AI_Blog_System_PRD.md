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

## 4. Scope (Enhanced Version)

This PRD covers the enhanced version of the AI-powered publishing system with interactive chat capabilities.

**In Scope:**

* **Local Development Environment:** DDEV for Drupal 11, Docker Compose for AI Agent.
* **Interactive Chat Interface:**
    * Command-line based chat interface for brainstorming and draft development.
    * Persistent chat sessions with unique identifiers and timestamps.
    * Chat history storage in local JSON/SQLite database.
    * Ability to resume previous chat sessions.
    * Export chat conversations to Markdown drafts.
    * Search and filter through chat history.
* **Article Generation:**
    * Accepts a Markdown-formatted draft (mix of Russian and English phrases, outlines, technical concepts, placeholder code) as input.
    * Accepts chat session exports as input for article generation.
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
    * Populates the article title and body (after **converting Markdown to HTML**) from the approved local file.
    * The HTML content for the body field will use a CKEditor-compatible text format (e.g., `full_html`).
    * Links the uploaded `media--image` entities to the article node via a configured entity reference field (images are **not embedded** within the body HTML).
    * Sets the article status to "Published."
    * Utilizes Basic Authentication for Drupal API communication (over HTTPS, as provided by DDEV).
* **Command-Line Interface:** Enhanced CLI with `chat`, `generate`, `publish`, and `history` commands.
* **Configuration:** API keys and Drupal credentials managed via environment variables (`.env` file).

**Out of Scope (for enhanced version):**

* Web user interface for the AI agent (CLI-focused approach maintained).
* Advanced content types or custom fields beyond basic article structure.
* Automatic SEO optimization suggestions or meta tag generation by AI.
* Multi-lingual article generation beyond English output.
* Automated social media posting upon article publication.
* Complex content scheduling.
* Automated re-generation or updates of existing articles.
* Deployment of the AI agent to a production server (initial focus is local development).
* Real-time collaborative chat features (single-user focused).

## 5. Functional Requirements

### 5.1. Interactive Chat System
* **REQ-CHAT-001:** The system SHALL provide a command-line chat interface for interactive idea development.
* **REQ-CHAT-002:** The system SHALL maintain conversation context throughout a chat session.
* **REQ-CHAT-003:** The system SHALL save chat sessions with unique identifiers (timestamp-based or user-defined names).
* **REQ-CHAT-004:** The system SHALL store chat history in a local database (JSON or SQLite).
* **REQ-CHAT-005:** The system SHALL allow users to resume previous chat sessions by ID or name.
* **REQ-CHAT-006:** The system SHALL provide commands to list all saved chat sessions.
* **REQ-CHAT-007:** The system SHALL allow searching chat history by keywords or date ranges.
* **REQ-CHAT-008:** The system SHALL export chat conversations to Markdown format for draft creation.
* **REQ-CHAT-009:** The system SHALL support mixed Russian/English input in chat conversations.
* **REQ-CHAT-010:** The system SHALL provide chat commands for session management (save, load, delete, export).

### 5.2. AI Agent Core
* **REQ-AI-001:** The system SHALL accept a Markdown file as input for article generation.
* **REQ-AI-002:** The system SHALL accept exported chat sessions as input for article generation.
* **REQ-AI-003:** The system SHALL process input drafts containing a mix of Russian and English phrases.
* **REQ-AI-004:** The system SHALL use the Google Gemini API to expand the draft into a full article.
* **REQ-AI-005:** The system SHALL support switching to OpenAI GPT API for article generation via configuration.
* **REQ-AI-006:** The generated article SHALL be entirely in English.
* **REQ-AI-007:** The generated article SHALL include accurate and properly formatted code snippets relevant to the technical content.
* **REQ-AI-008:** The generated article SHALL be structured with headings, paragraphs, and lists as appropriate for a technical blog post.
* **REQ-AI-009:** The system SHALL use the DALL-E API for image generation.
* **REQ-AI-010:** The generated images SHALL be illustrative or abstract in style.
* **REQ-AI-011:** The system SHALL save the generated article as a Markdown file in a designated local `output/` directory for review.
* **REQ-AI-012:** The system SHALL save generated images (e.g., PNG format) in the designated local `output/` directory.

### 5.3. Enhanced Workflow & User Interaction
* **REQ-WF-001:** The user SHALL initiate chat sessions via a command-line interface.
* **REQ-WF-002:** The user SHALL initiate article generation via CLI, specifying either an input draft file or a chat session ID.
* **REQ-WF-003:** The system SHALL provide a clear message prompting the user to review and edit the generated article and images locally.
* **REQ-WF-004:** The user SHALL manually edit the generated Markdown article and select/rename images as needed.
* **REQ-WF-005:** The user SHALL initiate the publishing process via a separate command-line command, referencing the *approved* local article file.
* **REQ-WF-006:** The system SHALL provide status updates during chat, generation, and publishing processes.
* **REQ-WF-007:** The system SHALL maintain a history of all chat sessions, generated articles, and published content.

### 5.4. Drupal Integration
* **REQ-DR-001:** The system SHALL authenticate with the Drupal 11 JSON:API using Basic Authentication (username/password of a dedicated API user).
* **REQ-DR-002:** The system SHALL first upload images to Drupal's file system via the `jsonapi/file/file` endpoint.
* **REQ-DR-003:** Upon successful file upload, the system SHALL create a `media--image` entity in Drupal, linking to the uploaded file's UUID.
* **REQ-DR-004:** The system SHALL create a new Drupal "Article" node via the `jsonapi/node/article` endpoint.
* **REQ-DR-005:** The system SHALL populate the "Article" node's `title` field from the first H1 heading of the approved Markdown file.
* **REQ-DR-006:** The system SHALL convert the approved Markdown content to HTML and populate the "Article" node's `body` field with this HTML, specifying a CKEditor-compatible text format (e.g., `full_html`).
* **REQ-DR-007:** The system SHALL link the created `media--image` entities to an entity reference field on the "Article" node (e.g., `field_images`). Images are explicitly **not embedded** within the body HTML content.
* **REQ-DR-008:** The published article in Drupal SHALL have its status set to "Published."

## 6. Non-Functional Requirements

* **Performance:** Chat responses should be near real-time (under 3 seconds), article and image generation should complete in reasonable timeframe (under 5 minutes), publishing should complete under 30 seconds.
* **Data Persistence:** Chat history and session data SHALL be stored locally and persist between application restarts.
* **Security:**
    * API keys and credentials SHALL be stored securely as environment variables (not hardcoded).
    * Chat history SHALL be stored locally and not transmitted to external services unless explicitly requested.
    * Communication with Drupal SHALL occur over HTTPS.
    * The Drupal API user SHALL have minimum necessary permissions.
* **Maintainability:** The Python codebase SHALL be modular, well-commented, and adhere to Python best practices, facilitating future enhancements.
* **Scalability (Local):** The containerized setup shall allow for easy replication and sharing of the development environment.
* **Reliability:** The system SHALL gracefully handle API errors (e.g., rate limits, invalid responses) and provide informative messages to the user.
* **Usability (for developer):** The command-line interface should be intuitive, provide clear instructions, and offer helpful chat commands.

## 7. Technical Specifications

* **Drupal Version:** Drupal 11
* **Local Drupal Environment:** DDEV
* **AI Agent Language:** Python 3.x
* **Containerization:** Docker, Docker Compose
* **LLM APIs:** Google Gemini API, OpenAI GPT API
* **Image Generation API:** OpenAI DALL-E API
* **Drupal API:** JSON:API (core module)
* **Data Storage:** SQLite for chat history, JSON for configuration
* **Python Libraries:** `requests`, `openai`, `google-generativeai`, `python-dotenv`, `markdown`, `sqlite3`, `click` (for enhanced CLI), `rich` (for enhanced terminal output).
* **Input/Output Formats:** Markdown for internal text generation/editing, PNG for images, HTML for Drupal body, JSON:API for Drupal communication, SQLite for chat storage.

## 8. Enhanced Command Structure

* **Chat Commands:**
    * `chat start [--name SESSION_NAME]` - Start a new chat session
    * `chat resume SESSION_ID` - Resume an existing chat session
    * `chat list` - List all saved chat sessions
    * `chat search KEYWORD` - Search chat history
    * `chat export SESSION_ID [--output FILENAME]` - Export chat to Markdown
    * `chat delete SESSION_ID` - Delete a chat session
* **Generation Commands:**
    * `generate --input-file DRAFT_FILE` - Generate from Markdown draft
    * `generate --chat-session SESSION_ID` - Generate from chat session
* **Publishing Commands:**
    * `publish --article-file ARTICLE_FILE` - Publish approved article
* **History Commands:**
    * `history chats` - Show chat session history
    * `history articles` - Show generated articles history
    * `history published` - Show published articles history

## 9. Future Considerations (Potential Enhancements)

* Integration with other open-source LLMs (e.g., self-hosted Llama variants, Mistral) for local inference.
* Implementation of RAG (Retrieval-Augmented Generation) for highly accurate technical details from a curated knowledge base.
* Web UI for chat interface with rich text editing capabilities.
* Export chat history to various formats (PDF, HTML, etc.).
* Integration with version control systems for draft management.
* Collaborative chat features for team environments.
* Advanced search capabilities with full-text indexing.
* Chat session branching and merging capabilities.
* AI-powered content summaries or SEO metadata generation.
* Automated social media sharing.
* Support for multiple image placements within an article.
* More sophisticated prompt extraction from drafts for image generation.

## 10. Success Criteria

* The system successfully provides an intuitive chat interface for idea development and draft creation.
* Chat sessions are reliably saved and can be resumed across application restarts.
* The system generates coherent and detailed English technical articles from both traditional drafts and chat session exports.
* The system consistently generates relevant illustrative/abstract images.
* Articles and images are published to the DDEV Drupal site correctly via API, requiring minimal manual post-publication adjustments.
* The overall workflow significantly reduces the time taken from initial idea to published content.
* The developer finds the enhanced system efficient, intuitive, and enjoyable to use.
* Chat history provides valuable reference material for future article development.