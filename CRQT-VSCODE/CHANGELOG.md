# Change Log

All notable changes to the "crqt-support" extension will be documented in this file.

Check [Keep a Changelog](http://keepachangelog.com/) for recommendations on how to structure this file.

## [Unreleased]

## [0.0.1] - 2026-02-09

### Added
- Initial release
- Syntax highlighting for CRQT format
- Support for 15 different tag types
- Language configuration for auto-closing pairs and bracket matching
- Example CRQT document
- CRQT Default Light theme

### Structure Tags (Block-level)
- `<as></as>` - Answer
- `<jx></jx>` - Parsing/Analysis
- `<opt></opt>` - Options
- `<qt></qt>` - Question Root Container
- `<tex></tex>` - Question Stem

### Style Tags (Inline)
- `<ilt></ilt>` - Italic
- `<str></str>` - Bold
- `<udl></udl>` - Underline

### Status Tags (Markers)
- `<fs>` - Font Size Toggle
- `<ft>` - Font Type Toggle

### Component Tags (Self-closing)
- `<cd#*>` - Fill-in-the-blank Line
- `<hx>` - Block-level Long Line
- `<p>` - Line Break
- `<pt>` - Image Path

### Character Tags (Self-closing)
- `<uni>` - Unicode Escape

## [0.0.0] - Project scaffolding

- Create initial project structure
