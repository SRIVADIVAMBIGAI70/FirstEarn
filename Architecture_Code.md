title Education & Income Support Platform

// Users
Users [icon: users, color: blue] {
  Students [icon: book-open, label: "School & College Students"]
  Elders [icon: heart, label: "Retired Teachers & Mentors"]
  Admin [icon: shield]
}

// Frontend
Frontend [icon: monitor, color: green] {
  Web App [icon: globe, label: "Web Interface"]
  
  Pages [icon: layout] {
    Schemes Page [icon: file-text, label: "Education Schemes"]
    Books Page [icon: book, label: "Book Listings"]
    Sessions Page [icon: video, label: "Learning Sessions"]
  }
}

// Backend
Backend [icon: server, color: purple] {
  API Server [icon: cpu, label: "API Server"]
  
  Services [icon: layers] {
    Auth Service [icon: lock, label: "Authentication"]
    User Service [icon: user, label: "User Management"]
    Scheme Service [icon: award, label: "Scheme Management"]
    Book Service [icon: book, label: "Book Management"]
    Session Service [icon: calendar, label: "Session Management"]
  }
}

// Database
Database [icon: database, color: orange] {
  Main DB [icon: aws-rds, label: "PostgreSQL"] {
    User Data [icon: users, label: "Students & Elders"]
    Scheme Data [icon: file-text, label: "Schemes"]
    Book Data [icon: book, label: "Books"]
    Session Data [icon: video, label: "Sessions"]
  }
}

// Connections
Students > Web App: access platform
Elders > Web App: teach & mentor
Admin > Web App: manage system

Web App > Schemes Page
Web App > Books Page
Web App > Sessions Page

Schemes Page > API Server
Books Page > API Server
Sessions Page > API Server

API Server > Auth Service
API Server > User Service
API Server > Scheme Service
API Server > Book Service
API Server > Session Service

Auth Service <> Main DB
User Service <> Main DB
Scheme Service <> Main DB
Book Service <> Main DB
Session Service <> Main DB