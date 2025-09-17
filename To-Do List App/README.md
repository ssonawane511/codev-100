# Microsoft To Do App - Project Rules
*Comprehensive project guidelines and development standards*

## 📋 Project Overview

This directory contains all the project rules and guidelines for building a Microsoft To Do-inspired task management application. These rules ensure consistency, maintainability, and quality throughout the development process.

## 📁 Project Rules Structure

### 🗂️ Core Project Rules
- **[project-structure.md](./project-structure.md)** - Directory organization and file placement guidelines
- **[design-guidelines.md](./design-guidelines.md)** - Visual design system and UI/UX standards
- **[project-scope.md](./project-scope.md)** - Project context, scope, and success criteria
- **[data-flow.md](./data-flow.md)** - State management and data flow architecture
- **[typescript-rules.md](./typescript-rules.md)** - Type safety and code contracts
- **[tech-stack.md](./tech-stack.md)** - Technology stack and dependencies specification

### 📋 Feature Specifications
- **[features/task-management.md](./features/task-management.md)** - Core task CRUD functionality
- **[features/task-lists.md](./features/task-lists.md)** - List organization and management
- **[features/user-interface.md](./features/user-interface.md)** - UI components and user experience

### 🚀 Implementation Guide
- **[development-plan.md](./development-plan.md)** - Step-by-step implementation checklist

## 🎯 Quick Reference

### Project Context
- **Who**: Individuals and small teams managing personal productivity
- **How**: Cross-platform web application with seamless sync
- **Why**: Reduce cognitive load and improve productivity

### Tech Stack
- **Frontend**: React 18+ with TypeScript 5+
- **Build Tool**: Vite for fast development and building
- **State Management**: Zustand for client state, React Query for server state
- **Styling**: Tailwind CSS with Headless UI components
- **Icons**: Lucide React icon library
- **Storage**: LocalStorage with future API integration
- **Testing**: Vitest + React Testing Library
- **Animation**: Framer Motion for smooth interactions

### Key Principles
1. **TypeScript First**: No code without types
2. **Single Source of Truth**: All data flows through stores
3. **Design System Consistency**: Every component follows the design system
4. **Accessibility First**: WCAG 2.1 AA compliance
5. **Mobile First**: Responsive design from the start

## 🚀 Getting Started

### Before Writing Code
1. **Read the Rules**: Familiarize yourself with all project rules
2. **Understand the Scope**: Review project-scope.md for context
3. **Set Up Types**: Define TypeScript contracts first
4. **Follow Structure**: Use the defined directory structure
5. **Design System**: Follow the design guidelines

### Development Workflow
1. **Plan Feature**: Create/update feature PRD
2. **Define Types**: Add TypeScript interfaces
3. **Update Store**: Add state management logic
4. **Build Components**: Follow design system
5. **Test Thoroughly**: Ensure quality and accessibility

## 📊 Success Metrics

### User Experience
- Task creation time < 30 seconds
- 70%+ task completion rate
- 99.9% uptime
- < 3 second page load time

### Code Quality
- 100% TypeScript coverage
- 90%+ test coverage
- Zero accessibility violations
- Consistent code style

## 🔄 Rule Updates

These rules are living documents that should be updated as the project evolves:

- **New Features**: Add feature PRDs before implementation
- **Architecture Changes**: Update data-flow.md and project-structure.md
- **Design Updates**: Modify design-guidelines.md
- **Type Changes**: Update typescript-rules.md and type definitions

## 📚 Additional Resources

### Design Inspiration
- [Microsoft Fluent Design](https://fluent2.microsoft.design/)
- [Microsoft To Do](https://todo.microsoft.com/)
- [Figma Design System](https://www.figma.com/community)

### Development Tools
- [React TypeScript Cheatsheet](https://react-typescript-cheatsheet.netlify.app/)
- [Tailwind CSS](https://tailwindcss.com/)
- [Zustand](https://github.com/pmndrs/zustand)
- [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/)

### Accessibility
- [WCAG 2.1 Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)
- [WebAIM](https://webaim.org/)
- [A11y Project](https://www.a11yproject.com/)

---

**Remember**: These rules exist to ensure quality, consistency, and maintainability. When in doubt, refer to the specific rule files for detailed guidance.

**Rule**: Every team member must read and understand these rules before contributing to the project. No exceptions.
