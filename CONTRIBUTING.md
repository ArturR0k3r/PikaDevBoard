# Contributing to PikaDevBoard

Thank you for your interest in contributing to PikaDevBoard! This document provides guidelines and information for contributors.

## 🤝 How to Contribute

### Types of Contributions

We welcome contributions in several areas:

- **🔧 Hardware improvements** - PCB design enhancements, new variants
- **💻 Software examples** - New example projects, libraries, utilities
- **📚 Documentation** - Guides, tutorials, API documentation
- **🐛 Bug fixes** - Hardware issues, software bugs, documentation errors
- **✨ New features** - Additional functionality, tool integrations
- **🧪 Testing** - Validation of examples, hardware testing

## 📋 Getting Started

### 1. Fork and Clone
```bash
# Fork the repository on GitHub, then clone your fork
git clone https://github.com/YOUR_USERNAME/PikaDevBoard.git
cd PikaDevBoard

# Add upstream remote
git remote add upstream https://github.com/ArturR0k3r/PikaDevBoard.git
```

### 2. Create a Branch
```bash
# Create a feature branch for your contribution
git checkout -b feature/your-feature-name

# Or for bug fixes
git checkout -b fix/issue-description
```

### 3. Make Your Changes
- Follow the coding standards outlined below
- Test your changes thoroughly
- Update documentation as needed
- Add examples if introducing new features

### 4. Submit a Pull Request
```bash
# Push your changes to your fork
git push origin feature/your-feature-name

# Open a pull request on GitHub
```

## 📝 Contribution Guidelines

### Code Style

#### C/C++ Code (STM32 Projects)
```c
// Use descriptive names
void ConfigureLED(GPIO_TypeDef* port, uint16_t pin);

// Comment complex logic
// Configure Timer 2 for 1kHz PWM generation
TIM2->PSC = 84000 - 1;  // Prescaler for 1kHz from 84MHz
TIM2->ARR = 1000 - 1;   // Auto-reload for 1ms period

// Use consistent formatting
if (condition) {
    DoSomething();
} else {
    DoSomethingElse();
}
```

#### Documentation (Markdown)
- Use clear, descriptive headings
- Include code examples where appropriate
- Add screenshots for GUI procedures
- Keep line length reasonable (80-100 characters)

### Hardware Contributions

#### PCB Design
- Use KiCad (preferred) or Eagle
- Include both source files and Gerber outputs
- Document design decisions in README
- Provide assembly drawings and pick-and-place files

#### Schematics
- Use standard symbols and conventions
- Include reference designators and values
- Add notes for critical components or connections
- Export to PDF for easy viewing

### Software Examples

#### Structure Requirements
Each software example should include:

```
example_name/
├── README.md              # Detailed explanation
├── example_name.ioc       # STM32CubeMX file
├── .project/.cproject     # STM32CubeIDE files
├── Core/
│   ├── Inc/
│   └── Src/
├── Drivers/               # HAL drivers
└── screenshots/           # Optional: GUI screenshots
```

#### README Template
```markdown
# Example Name

## Purpose
Brief description of what this example demonstrates.

## Hardware Required
- List required components
- Pin connections needed

## Expected Behavior  
What should happen when the example runs.

## Configuration Notes
Important setup details.

## Troubleshooting
Common issues and solutions.
```

## 🧪 Testing

### Hardware Testing
Before submitting hardware changes:

- [ ] **Design Rule Check** passes
- [ ] **Electrical Rule Check** passes  
- [ ] **3D visualization** looks correct
- [ ] **Gerber files** generated successfully
- [ ] **BOM export** works properly
- [ ] **Physical prototype** tested (if possible)

### Software Testing
Before submitting code:

- [ ] **Compiles** without warnings
- [ ] **Flashes** to target successfully  
- [ ] **Runs** as expected
- [ ] **Documentation** is accurate
- [ ] **Example works** on clean project
- [ ] **No HAL errors** or memory issues

## 📖 Documentation Standards

### README Files
- Start with clear purpose statement
- Include prerequisites and setup steps
- Provide troubleshooting section
- Add links to related examples
- Use consistent formatting

### Code Comments
```c
/**
 * @brief Configure PWM output for LED brightness control
 * @param brightness: PWM duty cycle (0-100)
 * @retval None
 */
void SetLEDBrightness(uint8_t brightness) {
    // Validate input range
    if (brightness > 100) brightness = 100;
    
    // Calculate timer compare value
    uint16_t compare_val = (brightness * TIM_PERIOD) / 100;
    
    // Update PWM duty cycle
    __HAL_TIM_SET_COMPARE(&htim2, TIM_CHANNEL_1, compare_val);
}
```

### Commit Messages
Use descriptive commit messages:

```bash
# Good examples:
git commit -m "Add UART example with interrupt handling"
git commit -m "Fix GPIO configuration in LED blink example"  
git commit -m "Update assembly guide with clearer photos"

# Avoid:
git commit -m "Update stuff"
git commit -m "Fix bug"
```

## 🚀 Submission Process

### Pull Request Checklist

Before submitting your pull request:

- [ ] **Code follows** style guidelines
- [ ] **Tests pass** (if applicable)
- [ ] **Documentation** is updated
- [ ] **CHANGELOG.md** is updated (for significant changes)
- [ ] **Commit messages** are clear and descriptive
- [ ] **No merge conflicts** with main branch

### Pull Request Template

When opening a PR, include:

```markdown
## Description
Brief description of changes.

## Type of Change
- [ ] Bug fix
- [ ] New feature  
- [ ] Documentation update
- [ ] Hardware improvement

## Testing
Describe how you tested your changes.

## Screenshots
If applicable, add screenshots.

## Checklist
- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Documentation updated
```

## 🏷️ Issue Reporting

### Bug Reports
When reporting bugs, include:

- **Environment**: OS, STM32CubeIDE version, STM32 part number
- **Steps to reproduce**: Exact steps that cause the issue
- **Expected behavior**: What should happen
- **Actual behavior**: What actually happens  
- **Additional context**: Screenshots, error messages, etc.

### Feature Requests
For feature requests, provide:

- **Use case**: Why is this feature needed?
- **Proposed solution**: How should it work?
- **Alternatives**: Other approaches considered
- **Additional context**: Examples, references, etc.

## 🎯 Development Setup

### Required Tools

#### Software Development
- STM32CubeIDE (latest version)
- Git
- Text editor with Markdown support
- Terminal/Command prompt

#### Hardware Development  
- KiCad (for PCB design)
- ST-Link V2 programmer
- Multimeter and basic test equipment
- PikaDevBoard for testing

### Local Development
```bash
# Keep your fork updated
git fetch upstream
git checkout main
git merge upstream/main

# Start new feature
git checkout -b feature/new-awesome-feature

# Regular commits as you work
git add .
git commit -m "Implement basic functionality"

# Push when ready for review
git push origin feature/new-awesome-feature
```

## 📞 Getting Help

### Community Support
- **GitHub Discussions** - General questions and ideas
- **GitHub Issues** - Bug reports and feature requests
- **Discord/Chat** - Real-time community discussion (if available)

### Maintainer Contact
- Open an issue for project-related questions
- Tag @ArturR0k3r for maintainer attention
- Be patient - maintainers are volunteers!

## 🏆 Recognition

Contributors will be recognized in:

- **Contributors section** in README.md
- **CHANGELOG.md** for significant contributions
- **Release notes** for major features
- **Hall of Fame** for outstanding contributions

## 📄 License Agreement

By contributing, you agree that:

- Your contributions will be licensed under the project's MIT License
- Hardware contributions will be under CERN Open Hardware License v2.0
- You have the right to submit your contributions
- Your contributions are your original work

## ❓ Questions?

Don't hesitate to ask questions! The maintainers and community are here to help:

- Open a **Discussion** for general questions
- Create an **Issue** for specific problems  
- Comment on existing **Pull Requests** for related topics

Thank you for helping make PikaDevBoard better! 🚀