# Frontend Workflow Diagrams

## Overview

This document contains Mermaid diagrams illustrating the key workflows and data flows in the Ribbit frontend application.

## Application Architecture Overview

```mermaid
graph TB
    subgraph "React Application"
        A[App.js] --> B[React Router]
        B --> C[Page Components]
        C --> D[Feature Components]
        D --> E[UI Components]
    end
    
    subgraph "State Management"
        F[Redux Store] --> G[Actions]
        G --> H[Reducers]
        H --> F
    end
    
    subgraph "Backend Integration"
        I[Axios HTTP Client] --> J[Django REST API]
        J --> K[Database]
    end
    
    C --> F
    G --> I
    
    style A fill:#e1f5fe
    style F fill:#f3e5f5
    style I fill:#e8f5e8
```

## User Authentication Flow

### Login Process

```mermaid
sequenceDiagram
    participant U as User
    participant L as Login Component
    participant A as Auth Action
    participant API as Backend API
    participant R as Redux Store
    participant LS as localStorage
    participant N as Navigation

    U->>L: Enter credentials
    L->>A: dispatch(login(email, password))
    A->>R: USER_LOGIN_REQUEST
    R-->>L: Show loading state
    
    A->>API: POST /api/auth/login/
    
    alt Success
        API-->>A: JWT tokens + user data
        A->>LS: Store userInfo
        A->>R: USER_LOGIN_SUCCESS
        R-->>L: Update UI with user data
        L->>N: Redirect to home
    else Failure
        API-->>A: Error response
        A->>R: USER_LOGIN_FAIL
        R-->>L: Show error message
    end
```

### User Registration Flow

```mermaid
sequenceDiagram
    participant U as User
    participant RC as Register Component
    participant A as Auth Action
    participant API as Backend API
    participant R as Redux Store

    U->>RC: Fill registration form
    RC->>A: dispatch(register(userData))
    A->>R: USER_REGISTER_REQUEST
    R-->>RC: Show loading state
    
    A->>API: POST /api/auth/register/
    
    alt Success
        API-->>A: User created successfully
        A->>R: USER_REGISTER_SUCCESS
        R-->>RC: Show success message
        RC->>RC: Redirect to login
    else Failure
        API-->>A: Validation errors
        A->>R: USER_REGISTER_FAIL
        R-->>RC: Show error messages
    end
```

## Post Management Workflows

### Post Creation Flow

```mermaid
graph TD
    A[User clicks Create Post] --> B[CreatePost Component]
    B --> C[Fill Post Form]
    C --> D[Select Community]
    D --> E[Submit Form]
    E --> F[Validate Input]
    
    F --> G{Valid?}
    G -->|No| H[Show Validation Errors]
    H --> C
    
    G -->|Yes| I[dispatch createPost]
    I --> J[CREATE_POST_REQUEST]
    J --> K[Show Loading State]
    I --> L[API Call: POST /api/post/create/]
    
    L --> M{Success?}
    M -->|Yes| N[CREATE_POST_SUCCESS]
    M -->|No| O[CREATE_POST_FAIL]
    
    N --> P[Update Redux Store]
    P --> Q[Show Success Toast]
    Q --> R[Redirect to Post Detail]
    
    O --> S[Show Error Message]
    S --> C
    
    style A fill:#e3f2fd
    style R fill:#e8f5e8
    style S fill:#ffebee
```

### Post Voting Flow

```mermaid
sequenceDiagram
    participant U as User
    participant V as VoteButton
    participant A as Vote Action
    participant API as Backend API
    participant R as Redux Store
    participant P as Post Component

    U->>V: Click upvote/downvote
    V->>A: dispatch(votePost(postId, voteType))
    A->>R: VOTE_POST_REQUEST
    R-->>V: Show loading state
    
    A->>API: POST /api/vote/
    
    alt Success
        API-->>A: Updated vote data
        A->>R: VOTE_POST_SUCCESS
        R-->>P: Update post vote count
        R-->>V: Update vote button state
    else Failure
        API-->>A: Error response
        A->>R: VOTE_POST_FAIL
        R-->>V: Show error, revert state
    end
```

## Comment System Workflows

### Comment Creation Flow

```mermaid
graph TD
    A[User types comment] --> B[Comment Form]
    B --> C[Submit Comment]
    C --> D[dispatch createComment]
    D --> E[CREATE_COMMENT_REQUEST]
    E --> F[API: POST /api/comment/create/]
    
    F --> G{Success?}
    G -->|Yes| H[CREATE_COMMENT_SUCCESS]
    G -->|No| I[CREATE_COMMENT_FAIL]
    
    H --> J[Add comment to Redux]
    J --> K[Update comment count]
    K --> L[Clear comment form]
    L --> M[Show success feedback]
    
    I --> N[Show error message]
    N --> B
    
    style A fill:#e3f2fd
    style M fill:#e8f5e8
    style N fill:#ffebee
```

### Comment Like/Unlike Flow

```mermaid
sequenceDiagram
    participant U as User
    participant L as LikeButton
    participant A as Comment Action
    participant API as Backend API
    participant R as Redux Store

    U->>L: Click like button
    L->>A: dispatch(likeUnlikeComment(commentId))
    A->>R: LIKE_UNLIKE_REQUEST
    
    A->>API: POST /api/comment/like-unlike/
    
    alt Success
        API-->>A: Updated like status
        A->>R: LIKE_UNLIKE_SUCCESS
        R-->>L: Update like count & button state
    else Failure
        API-->>A: Error response
        A->>R: LIKE_UNLIKE_FAIL
        R-->>L: Show error message
    end
```

## Community (Subribbit) Workflows

### Community Creation Flow

```mermaid
graph LR
    A[User clicks Create Community] --> B[CreateSubribbit Form]
    B --> C[Fill Community Details]
    C --> D[Submit Form]
    D --> E[Validation]
    
    E --> F{Valid?}
    F -->|No| G[Show Errors]
    G --> C
    
    F -->|Yes| H[dispatch createSubribbit]
    H --> I[API Call]
    I --> J{Success?}
    
    J -->|Yes| K[Community Created]
    K --> L[Redirect to Community]
    
    J -->|No| M[Show Error]
    M --> C
    
    style A fill:#e3f2fd
    style L fill:#e8f5e8
    style M fill:#ffebee
```

### Community Join Flow

```mermaid
sequenceDiagram
    participant U as User
    participant S as Subribbit Component
    participant A as Subribbit Action
    participant API as Backend API
    participant R as Redux Store

    U->>S: Click Join Community
    S->>A: dispatch(requestJoinSubribbit(subribbitId))
    A->>R: REQUEST_JOIN_SUBRIBBIT_REQUEST
    R-->>S: Show loading state
    
    A->>API: POST /api/subribbit/join/{id}/
    
    alt Success
        API-->>A: Membership confirmed
        A->>R: REQUEST_JOIN_SUBRIBBIT_SUCCESS
        R-->>S: Update join button to "Joined"
        R-->>S: Update member count
    else Failure
        API-->>A: Error (e.g., already member)
        A->>R: REQUEST_JOIN_SUBRIBBIT_FAIL
        R-->>S: Show error message
    end
```

## Data Loading Patterns

### Page Load Data Flow

```mermaid
graph TD
    A[Component Mounts] --> B[useEffect Hook]
    B --> C[Check if data needed]
    C --> D{Data in Redux?}
    
    D -->|Yes| E[Use existing data]
    D -->|No| F[Dispatch fetch action]
    
    F --> G[REQUEST action]
    G --> H[Show loading state]
    F --> I[API call]
    
    I --> J{Success?}
    J -->|Yes| K[SUCCESS action]
    J -->|No| L[FAIL action]
    
    K --> M[Update Redux store]
    M --> N[Component re-renders]
    
    L --> O[Store error message]
    O --> P[Show error UI]
    
    E --> N
    
    style A fill:#e3f2fd
    style N fill:#e8f5e8
    style P fill:#ffebee
```

### Search Flow

```mermaid
sequenceDiagram
    participant U as User
    participant S as Search Component
    participant A as Action Creator
    participant API as Backend API
    participant R as Redux Store
    participant L as List Component

    U->>S: Type in search box
    S->>S: Debounce input
    S->>A: dispatch(searchPosts(query))
    A->>R: POST_LIST_REQUEST
    R-->>L: Show loading state
    
    A->>API: GET /api/post/all/home/?search=query
    
    alt Results found
        API-->>A: Filtered posts
        A->>R: POST_LIST_SUCCESS
        R-->>L: Display search results
    else No results
        API-->>A: Empty array
        A->>R: POST_LIST_SUCCESS
        R-->>L: Show "No results found"
    else Error
        API-->>A: Error response
        A->>R: POST_LIST_FAIL
        R-->>L: Show error message
    end
```

## Error Handling Workflows

### Global Error Handling

```mermaid
graph TD
    A[API Call] --> B{Response Status}
    B -->|200-299| C[Success Path]
    B -->|400| D[Client Error]
    B -->|401| E[Unauthorized]
    B -->|403| F[Forbidden]
    B -->|404| G[Not Found]
    B -->|500| H[Server Error]
    B -->|Network| I[Network Error]
    
    C --> J[Dispatch SUCCESS]
    D --> K[Show Validation Errors]
    E --> L[Redirect to Login]
    F --> M[Show Access Denied]
    G --> N[Show Not Found]
    H --> O[Show Server Error]
    I --> P[Show Network Error]
    
    K --> Q[Dispatch FAIL]
    L --> Q
    M --> Q
    N --> Q
    O --> Q
    P --> Q
    
    J --> R[Update UI]
    Q --> S[Show Error Message]
    
    style C fill:#e8f5e8
    style D fill:#fff3e0
    style E fill:#ffebee
    style F fill:#ffebee
    style G fill:#fff3e0
    style H fill:#ffebee
    style I fill:#ffebee
```

## Navigation and Routing

### Application Navigation Flow

```mermaid
graph TD
    A[App Start] --> B{User Authenticated?}
    B -->|No| C[Landing Page]
    B -->|Yes| D[Home Page]
    
    C --> E[Login/Register]
    E --> F{Auth Success?}
    F -->|Yes| D
    F -->|No| C
    
    D --> G[Main Navigation]
    G --> H[Home Feed]
    G --> I[Explore Communities]
    G --> J[User Profile]
    G --> K[Create Post]
    G --> L[Community Pages]
    
    H --> M[Post Details]
    I --> N[Community List]
    J --> O[User Posts]
    K --> P[Post Form]
    L --> Q[Community Feed]
    
    M --> R[Comments]
    N --> L
    O --> M
    P --> M
    Q --> M
    
    style A fill:#e3f2fd
    style D fill:#e8f5e8
    style G fill:#f3e5f5
```

## State Management Flow

### Redux Data Flow

```mermaid
graph LR
    A[User Interaction] --> B[Component]
    B --> C[Action Creator]
    C --> D[Action Dispatch]
    D --> E[Middleware: Thunk]
    E --> F[Async Operations]
    F --> G[API Calls]
    G --> H[Response]
    H --> I[Action Dispatch]
    I --> J[Reducers]
    J --> K[Store Update]
    K --> L[Component Re-render]
    L --> M[UI Update]
    
    style A fill:#e3f2fd
    style G fill:#e8f5e8
    style K fill:#f3e5f5
    style M fill:#e8f5e8
```

### Component State vs Redux State

```mermaid
graph TD
    A[Component State] --> B[Form Inputs]
    A --> C[UI State]
    A --> D[Temporary Data]
    
    E[Redux State] --> F[User Authentication]
    E --> G[API Data]
    E --> H[Global UI State]
    E --> I[Shared State]
    
    J[Component] --> K{State Type?}
    K -->|Local| A
    K -->|Global| E
    
    style A fill:#e3f2fd
    style E fill:#f3e5f5
    style J fill:#fff3e0
```

## Performance Optimization Workflows

### Lazy Loading Flow

```mermaid
sequenceDiagram
    participant U as User
    participant R as Router
    participant L as Lazy Component
    participant B as Bundle
    participant C as Component

    U->>R: Navigate to route
    R->>L: Load component
    L->>B: Request bundle
    B-->>L: Bundle loaded
    L->>C: Render component
    C-->>U: Display content
    
    Note over L,B: Component bundle loaded on demand
```

### Component Re-render Optimization

```mermaid
graph TD
    A[Redux State Change] --> B[Connected Components]
    B --> C{State Changed?}
    C -->|Yes| D[Component Re-render]
    C -->|No| E[Skip Re-render]
    
    D --> F[Child Components]
    F --> G{Props Changed?}
    G -->|Yes| H[Child Re-render]
    G -->|No| I[Skip Child Re-render]
    
    E --> J[Performance Optimized]
    I --> J
    H --> K[Update UI]
    
    style E fill:#e8f5e8
    style I fill:#e8f5e8
    style J fill:#e8f5e8
```

## Mobile Responsiveness Flow

### Responsive Design Workflow

```mermaid
graph TD
    A[User Device] --> B{Screen Size?}
    B -->|Mobile| C[Mobile Layout]
    B -->|Tablet| D[Tablet Layout]
    B -->|Desktop| E[Desktop Layout]
    
    C --> F[Collapsed Navigation]
    C --> G[Single Column]
    C --> H[Touch Optimized]
    
    D --> I[Adapted Navigation]
    D --> J[Two Column]
    D --> K[Touch + Mouse]
    
    E --> L[Full Navigation]
    E --> M[Multi Column]
    E --> N[Mouse Optimized]
    
    F --> O[Responsive UI]
    G --> O
    H --> O
    I --> O
    J --> O
    K --> O
    L --> O
    M --> O
    N --> O
    
    style A fill:#e3f2fd
    style O fill:#e8f5e8
```

---

*These diagrams illustrate the key workflows and data flows in the Ribbit frontend application. They can be used to understand component interactions, state management patterns, and user journey flows.*