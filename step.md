# AI-Powered Drupal Blog Publishing System - Step-by-Step Implementation

## Phase 1: Environment Setup

### 1.1 Prerequisites
- [ ] Install Docker Desktop
- [ ] Install DDEV
- [ ] Install Python 3.11+
- [ ] Obtain Google Gemini API key
- [ ] Obtain OpenAI API key (for DALL-E image generation)

### 1.2 Drupal 11 Setup with DDEV
- [ ] Create project directory: `mkdir drupal-blog-agent && cd drupal-blog-agent`
- [ ] Initialize DDEV configuration: `ddev config --project-type=drupal11`
- [ ] Create Drupal 11 site: `ddev composer create drupal/recommended-project`
- [ ] Install Drupal: `ddev drush site:install`
- [ ] Enable required modules: `ddev drush en jsonapi rest serialization media`
- [ ] Create dedicated API user `ai_publisher` with permissions:
    - Create content (Article)
    - Create media
    - Upload files
    - Access JSON:API
- [ ] Configure Article content type with image field (`field_images`)

### 1.3 AI Agent Setup
- [ ] Create `ai-agent/` directory structure
- [ ] Create Docker environment with Python 3.11+
- [ ] Set up environment variables configuration (`.env` file)
- [ ] Create project structure:
  ```
  ai-agent/
  ├── main.py
  ├── config/
  ├── services/
  ├── utils/
  ├── requirements.txt
  ├── Dockerfile
  └── docker-compose.yml
  ```

## Phase 2: Core AI Services Implementation

### 2.1 Configuration Management
- [ ] Create environment variable handler
- [ ] Implement API key validation
- [ ] Create configuration class for Drupal connection settings
- [ ] Set up logging configuration

### 2.2 Text Generation Service
- [ ] Implement Google Gemini API integration
- [ ] Implement OpenAI GPT API integration
- [ ] Create API selector (configurable choice between Gemini/GPT)
- [ ] Implement prompt engineering for:
    - Processing mixed Russian/English drafts
    - Generating structured English technical articles
    - Including proper code snippets
    - Creating appropriate headings and formatting

### 2.3 Image Generation Service
- [ ] Implement DALL-E API integration
- [ ] Create prompt generation from article content
- [ ] Implement image download and local storage
- [ ] Add support for PNG format output
- [ ] Create image naming convention

### 2.4 Content Processing Utilities
- [ ] Implement Markdown parser
- [ ] Create Markdown to HTML converter (CKEditor compatible)
- [ ] Implement title extraction from H1 headings
- [ ] Create file management utilities for local storage

## Phase 3: Drupal Integration

### 3.1 Authentication System
- [ ] Implement Basic Authentication for Drupal JSON:API
- [ ] Create secure credential management
- [ ] Add HTTPS support validation
- [ ] Implement authentication error handling

### 3.2 File Upload Service
- [ ] Implement file upload to `/jsonapi/file/file` endpoint
- [ ] Create file entity management
- [ ] Add file validation and error handling
- [ ] Implement retry logic for failed uploads

### 3.3 Media Entity Service
- [ ] Create media entity creation via `/jsonapi/media/image` endpoint
- [ ] Link uploaded files to media entities
- [ ] Implement media entity validation
- [ ] Add error handling for media creation

### 3.4 Article Node Service
- [ ] Implement article creation via `/jsonapi/node/article` endpoint
- [ ] Populate title field from Markdown H1
- [ ] Convert and populate body field with HTML content
- [ ] Link media entities to `field_images`
- [ ] Set article status to "Published"
- [ ] Add comprehensive error handling

## Phase 4: Command Line Interface

### 4.1 CLI Framework
- [ ] Implement argument parsing (argparse or click)
- [ ] Create main command dispatcher
- [ ] Add help documentation and usage examples
- [ ] Implement progress indicators and status messages

### 4.2 Generate Command
- [ ] Create `generate` command implementation
- [ ] Add input file validation
- [ ] Implement draft processing workflow:
    - Read Markdown input
    - Generate article via LLM
    - Generate images via DALL-E
    - Save outputs to local directory
- [ ] Add user feedback and status updates

### 4.3 Publish Command
- [ ] Create `publish` command implementation
- [ ] Add approval workflow confirmation
- [ ] Implement publishing pipeline:
    - Upload images to Drupal
    - Create media entities
    - Create article node
    - Link images to article
- [ ] Add success/failure reporting

## Phase 5: Error Handling & Validation

### 5.1 API Error Handling
- [ ] Implement rate limit handling for all APIs
- [ ] Add retry logic with exponential backoff
- [ ] Create informative error messages
- [ ] Handle API quota exceeded scenarios

### 5.2 Content Validation
- [ ] Validate Markdown input format
- [ ] Check generated content quality
- [ ] Validate image dimensions and format
- [ ] Ensure HTML output is CKEditor compatible

### 5.3 Drupal Integration Validation
- [ ] Validate Drupal API connectivity
- [ ] Check user permissions
- [ ] Validate content type and field configuration
- [ ] Test media library functionality

## Phase 6: Local Development Workflow

### 6.1 Directory Structure
- [ ] Create `drafts/` directory for input files
- [ ] Create `output/` directory for generated content
- [ ] Set up proper file permissions
- [ ] Add `.gitignore` for sensitive files

### 6.2 Docker Configuration
- [ ] Create Dockerfile for AI agent
- [ ] Set up docker-compose.yml
- [ ] Configure volume mounts for file access
- [ ] Add environment variable management

### 6.3 Development Tools
- [ ] Add logging configuration
- [ ] Create debugging utilities
- [ ] Implement dry-run mode for testing
- [ ] Add configuration validation tools

## Phase 7: Testing & Quality Assurance

### 7.1 Unit Testing
- [ ] Test configuration management
- [ ] Test API integrations (with mocks)
- [ ] Test content processing utilities
- [ ] Test CLI command parsing

### 7.2 Integration Testing
- [ ] Test full generation workflow
- [ ] Test publishing workflow
- [ ] Test error scenarios
- [ ] Test with various input formats

### 7.3 Manual Testing
- [ ] Test with mixed Russian/English drafts
- [ ] Verify article quality and formatting
- [ ] Test image generation relevance
- [ ] Validate published content in Drupal

## Phase 8: Documentation & Polish

### 8.1 User Documentation
- [ ] Create comprehensive README
- [ ] Add usage examples
- [ ] Document configuration options
- [ ] Create troubleshooting guide

### 8.2 Code Documentation
- [ ] Add docstrings to all functions
- [ ] Create API documentation
- [ ] Document configuration options
- [ ] Add inline comments for complex logic

### 8.3 Final Polish
- [ ] Code review and refactoring
- [ ] Performance optimization
- [ ] Security review
- [ ] User experience improvements

## Success Criteria Validation

- [ ] Generate coherent English articles from mixed-language drafts
- [ ] Consistently produce relevant illustrative images
- [ ] Successfully publish articles to DDEV Drupal site via API
- [ ] Significantly reduce time from idea to published content
- [ ] Provide efficient and enjoyable developer experience

## Future Enhancement Considerations

- [ ] Integration with other open-source LLMs
- [ ] RAG implementation for technical accuracy
- [ ] Web UI development
- [ ] Scheduled content generation
- [ ] SEO metadata generation
- [ ] Social media integration
- [ ] Multi-image placement support