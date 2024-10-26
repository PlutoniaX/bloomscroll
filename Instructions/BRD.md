# BloomScroll - Product Requirements Document
**Fighting doomscrolling with insightful, actionable content**

## Table of Contents
1. [Project Overview](#project-overview)
2. [Design Principles & System](#design-principles--system)
3. [Technical Architecture](#technical-architecture)
4. [Core Features](#core-features)
5. [User Interface Specifications](#user-interface-specifications)
6. [User Interaction Flow](#user-interaction-flow)
7. [Technical Requirements](#technical-requirements)
8. [API Integration Specifications](#api-integration-specifications)
9. [Data Models](#data-models)
10. [File Structure & Component Architecture](#file-structure--component-architecture)

## Project Overview

### Vision Statement
BloomScroll transforms diverse content sources into a familiar, social media-like experience that promotes meaningful engagement over mindless scrolling. Users can discover, save, and deeply engage with high-quality, actionable content through an intuitive card-based interface.

### Target Audience
- Knowledge workers seeking efficient content consumption
- Users trying to break free from traditional social media habits
- Learners who prefer bite-sized, actionable information

### Core Value Proposition
Transform lengthy content into digestible, actionable cards while maintaining depth through AI-powered interactions.

## Design Principles & System

### Design Philosophy
Following Dieter Rams' principles of good design, BloomScroll emphasizes:

1. **Innovation Through Reduction**
- Focus on core content consumption without artificial engagement metrics
- Remove all unnecessary UI elements that don't directly serve content understanding
- Eliminate "pull-to-refresh" addiction by using smart content preloading

### Core Design Values
1. **Honesty**: Clear indication of content source, length, and type
2. **Longevity**: Design patterns that discourage mindless consumption
3. **Environmental Care**: Dark mode default, reduced data usage
4. **Thoroughness**: Every detail serves the core purpose
5. **Unobtrusive**: Interface elements appear only when needed

### Visual Design System

#### Typography
```
Font Family: Inter
- Headlines: Inter Light (300)
- Body: Inter Regular (400)
- UI Elements: Inter Medium (500)

Scale:
- xs: 12px/16px
- sm: 14px/20px
- base: 16px/24px
- lg: 18px/28px
- xl: 20px/32px
- 2xl: 24px/36px

Letter-spacing:
- Headlines: -0.02em
- Body: 0
- UI: 0.01em
```

#### Color System
```
Primary Palette:
- Surface: #FFFFFF (Light), #121212 (Dark)
- Text: #1A1A1A (Light), #E9E9E9 (Dark)
- Accent: #3B82F6 (Blue)

Functional Colors:
- Success: #22C55E
- Archive: #6B7280
- Error: #EF4444

Interaction States:
- Hover: 8% opacity change
- Active: 12% opacity change
- Disabled: 40% opacity
```

#### Space & Layout
```
Grid:
- Base unit: 4px
- Content margins: 16px (mobile), 24px (tablet), 32px (desktop)
- Maximum content width: 600px
- Card aspect ratio: 3:4 (collapsed), flexible (expanded)

Spacing Scale:
- xs: 4px
- sm: 8px
- md: 16px
- lg: 24px
- xl: 32px
- 2xl: 48px
```

#### Motion Design
```
Timing Functions:
- Standard: cubic-bezier(0.4, 0.0, 0.2, 1)
- Decelerate: cubic-bezier(0.0, 0.0, 0.2, 1)
- Accelerate: cubic-bezier(0.4, 0.0, 1, 1)

Durations:
- Instant: 100ms
- Quick: 200ms
- Standard: 300ms
- Complex: 400ms

Properties:
- Transform origin: center
- Scale range: 0.97 to 1.02
- Opacity range: 0.4 to 1
```

## Technical Architecture

### Tech Stack
- **Frontend Framework**: Next.js 15
  - Server-side rendering for optimal performance
  - App Router for modern routing capabilities
- **Styling**: Tailwind CSS + shadcn/ui
  - Consistent design system
  - Mobile-first responsive design
- **Authentication**: Clerk
  - Social login integration
  - Session management
- **AI Integration**: 
  - Google Gemini API for content processing and chat
  - Groq Whisper API for audio transcription
- **Backend**: 
  - Supabase for database and storage
  - Next.js API routes for backend logic

## Core Features

### 1. Content Card System

#### Card Interaction Gestures
- **Swipe Right**: Save for later
- **Swipe Left**: Archive
- **Swipe Up**: Open chat interface
- **Pull Down**: Refresh feed
- **Tap**: View detailed summary

#### Card Content (front)
- Key insights (bullet points)
- Source reference (with hyperlink)
- Time to read
- Original content type indicator (video, audio, text, etc.)

#### Card Content (back)
- Detailed summary of the content
- Source reference (with hyperlink)

### 2. Content Processing Pipeline

#### Supported Content Types
- YouTube videos (via transcription)
- Audio podcasts
- PDF documents
- Articles (via URL)
- Text files
- eBooks

#### Processing Flow
1. Content ingestion
2. Format-specific processing
3. AI-powered insight extraction
4. Card generation
5. Related content linking

### 3. Chat Interface

#### Features
- Context-aware responses based on card content
- Ability to ask follow-up questions
- Source reference preservation
- Chat history persistence

## User Interface Specifications

### Content Card Component

#### Visual Design
```
Collapsed State Structure:
┌────────────────────────┐
│ Source • Time to read  │
├────────────────────────┤
│                        │
│    Primary Insight     │
│                        │
├────────────────────────┤
│ Type      Action Hints │
└────────────────────────┘

Dimensions:
- Height: 160px (minimum)
- Width: 100% (max 600px)
- Padding: 16px
- Border radius: 12px
```

#### Expanded State
```
Structure:
┌────────────────────────┐
│ Source • Time • Date   │
├────────────────────────┤
│                        │
│    Content Summary     │
│                        │
├────────────────────────┤
│    Key Takeaways       │
├────────────────────────┤
│   Related Content      │
└────────────────────────┘

Transitions:
- Expand duration: 400ms
- Content fade-in: 200ms delayed
```

#### Component States
- Default: Neutral background, standard elevation
- Hover: Subtle increase in elevation, slight scale transform
- Active/Dragging: Increased elevation, rotation based on drag direction
- Expanded: Smooth transition to full-width view with additional content
- Loading: Animated skeleton state with placeholder content

#### Gesture Implementation
```
Threshold Values:
- Swipe activation: 40px
- Velocity threshold: 0.5px/ms
- Rotation maximum: 12deg

Feedback:
- Haptic: Light tap (iOS), Vibration (Android)
- Visual: Color shift + rotation
- Audio: None (unnecessary)
```

### Chat Interface

#### Design Philosophy
- Preserve context while enabling depth
- Minimize UI chrome
- Focus on content over chat mechanics

#### Layout
```
Structure:
┌────────────────────────┐
│ Original Content       │
│ (Minimized)           │
├────────────────────────┤
│                        │
│    Chat Messages       │
│                        │
├────────────────────────┤
│ Input + Actions        │
└────────────────────────┘

Spacing:
- Message gap: 12px
- Section padding: 16px
- Input height: 56px
```

## User Interaction Flow

### Initial User Journey

#### 1. Onboarding
1. User arrives at landing page
   - Presented with value proposition and demo video
   - Clear CTA for sign-up/sign-in
2. Authentication flow (via Clerk)
   - Email/password or social login options
   - Optional profile customization
3. Content preference selection
   - Topic categories (e.g., Technology, Science, Business)
   - Content type preferences (Articles, Videos, Podcasts)
   - Reading time preferences (5-10 min, 10-20 min, etc.)

#### 2. Main Feed Introduction
1. Tutorial overlay highlights key interactions
   - Demonstrates swipe gestures
   - Shows card expansion
   - Explains chat functionality
2. Initial feed population
   - Mix of content types based on preferences
   - "Welcome" card with quick tips
   - Sample interactions with pre-loaded content

### Content Discovery & Interaction

#### 1. Feed Navigation
1. Pull-to-refresh mechanism
   - Loading animation
   - New content cards appear with fade-in
2. Infinite scroll implementation
   - Lazy loading of content
   - Smooth loading states
   - Background prefetching

#### 2. Card Interactions
1. Basic viewing
   - Tap to expand card
   - Scroll through summary
   - View source link
2. Gesture-based actions
   - Swipe right: Save content
     - Visual feedback (green checkmark)
     - Haptic feedback
     - Animation to saved list
   - Swipe left: Archive
     - Visual feedback (archive icon)
     - Removal animation
   - Swipe up: Open chat
     - Smooth transition to chat interface
     - Context preservation

### Deep Engagement Flow

#### 1. Content Expansion
1. Card expansion interaction
   - Smooth animation to full view
   - Background blur effect
   - Additional content appears
2. Extended content options
   - Full summary view
   - Key insights section
   - Source attribution
   - Related content suggestions

#### 2. Chat Interaction
1. Chat initiation
   - Context-aware greeting
   - Suggested questions displayed
   - Input field focus
2. Conversation flow
   - Real-time message updates
   - Typing indicators
   - Error state handling
   - Source references maintained
3. Follow-up actions
   - Save chat highlights
   - Share insights
   - Return to card view

### Content Management

#### 1. Saved Content
1. Access saved items
   - Grid/list view toggle
   - Sort/filter options
   - Search functionality
2. Organization features
   - Create collections
   - Add tags
   - Bulk actions

#### 2. Archive Management
1. Archive access
   - View archived content
   - Restore options
   - Permanent deletion
2. Archive features
   - Filter by date
   - Search archived content
   - Bulk restore/delete

### Content Addition Flow

#### 1. Content Import
1. Import initiation
   - URL paste
   - File upload
   - Share extension
2. Processing feedback
   - Progress indicator
   - Format detection
   - Error handling
3. Success state
   - Preview card
   - Edit options
   - Share options

#### 2. Custom Collections
1. Collection creation
   - Name input
   - Description
   - Privacy settings
2. Content organization
   - Drag-and-drop interface
   - Order customization
   - Sharing options

### Error & Edge Cases

#### 1. Error States
1. Network issues
   - Offline mode activation
   - Cached content access
   - Sync when online
2. Processing errors
   - Friendly error messages
   - Retry options
   - Alternative suggestions

#### 2. Empty States
1. Fresh start
   - Welcoming message
   - Suggested actions
   - Tutorial access
2. No results
   - Clear feedback
   - Alternative suggestions
   - Help resources

### Performance Considerations

#### 1. Loading States
- Skeleton screens for cards
- Progressive image loading
- Background data prefetching
- Smooth transitions

#### 2. Optimization
- Viewport-based rendering
- Image optimization
- Cached responses
- Debounced actions

## Technical Requirements

### Mobile Responsiveness
- Support all screen sizes (320px and up)
- Optimize touch interactions
- Adapt layout for different orientations
- Handle offline capabilities

### Accessibility & Inclusivity

#### Standards Compliance
- WCAG 2.1 Level AA
- Support for dynamic type
- Voice-over optimization
- RTL language support

#### Keyboard Navigation
```
Shortcuts:
- j/k: Navigate cards
- s: Save
- a: Archive
- c: Open chat
- Esc: Close expanded view
```

### Performance Guidelines

#### Image Optimization
```
Format Requirements:
- AVIF with WebP fallback
- Responsive srcset
- Lazy loading
- Low-quality placeholder
```

#### Animation Performance
```
Best Practices:
- Use transform over position
- Composite properties only
- GPU acceleration for cards
- Request animation frame
```

#### Loading Strategy
```
Priority Order:
1. Core UI shell
2. Content metadata
3. Card previews
4. Full content
5. Related items
```

## API Integration Specifications

### Google Gemini API 
- Use for content summarization
- Generate actionable insights
- Power chat interactions
- Handle rate limiting and quotas

### Groq Whisper API
- Audio file transcription
- Multiple language support
- Timestamp preservation
- Error handling for low-quality audio

### Supabase Integration
- User data storage
- Content metadata
- Chat history
- File storage for processed content

## Data Models

### Content Storage
```typescript
interface ProcessedContent {
  id: string;
  originalUrl: string;
  processedDate: Date;
  cards: ContentCard[];
  metadata: {
    sourceType: string;
    processingStatus: string;
    errorLog?: string[];
  };
}
```

## File Structure & Component Architecture

### Directory Structure
```
.
├── app/
│   ├── (auth)/
│   │   ├── sign-in/page.tsx        # Clerk authentication pages
│   │   └── sign-up/page.tsx
│   ├── api/
│   │   ├── chat/route.ts           # Chat endpoint
│   │   ├── process-content/route.ts # Content processing
│   │   └── transcribe/route.ts     # Audio transcription
│   ├── feed/
│   │   ├── archive/page.tsx        # Archived content view
│   │   ├── saved/page.tsx          # Saved content view
│   │   └── page.tsx                # Main feed
│   ├── layout.tsx
│   └── page.tsx
├── components/
│   ├── card/
│   │   ├── content-card.tsx        # Main card component
│   │   ├── card-actions.tsx        # Card interaction buttons
│   │   └── card-detail.tsx         # Expanded card view
│   ├── chat/
│   │   ├── chat-interface.tsx      # Chat UI component
│   │   └── message-bubble.tsx      # Individual message
│   ├── layout/
│   │   ├── header.tsx              # App header
│   │   └── navigation.tsx          # Navigation menu
│   └── ui/                         # shadcn components
├── lib/
│   ├── api/                        # API clients
│   ├── types/                      # TypeScript definitions
│   ├── utils/                      # Utility functions
│   └── hooks/                      # Custom React hooks
├── public/
├── styles/
└── config/
```

### Key Components Documentation

#### ContentCard
Primary card component handling:
- Gesture recognition
- Content display
- Animation states
- Interaction feedback

#### ChatInterface
Chat implementation featuring:
- Real-time message updates
- Context preservation
- Error handling
- Loading states

#### ContentProcessor
Content processing pipeline:
- Format detection
- Content extraction
- AI processing
- Card generation

This PRD provides a comprehensive guide for developers to understand and implement the BloomScroll project. Each section includes specific requirements, interfaces, and architectural decisions to ensure consistent development across the team