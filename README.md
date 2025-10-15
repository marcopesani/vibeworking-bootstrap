# Vibeworking Bootstrap

A comprehensive learning repository for understanding AI concepts, designed for designers and non-technical decision makers who want to master generative AI without diving into heavy mathematics.

## 🎯 What is this?

This repository serves as a bootstrap template for the **Vibeworking workshop** - an educational initiative focused on making AI concepts accessible to creative professionals and business leaders. The materials are designed to bridge the gap between technical AI concepts and practical application.

## 📚 Learning Materials

The repository contains four comprehensive guides that cover essential AI concepts:

### 1. [Understanding Denoising](./learn/denoise.md)
**From Noise to Vision: Understanding Denoising and Prompt Crafting in AI Image Generation**

- Learn why denoising is the heart of image generation
- Understand the architecture of diffusion models (UNet, VAE, Text Encoder)
- Master prompt design principles for better AI image generation
- Explore different generation modes (text-to-image, image-to-image, inpainting)

### 2. [Large Language Models Explained](./learn/llm.md)
**Understanding Large Language Models: How Machines Learn, Think, and Speak**

- From words to tokens: how text becomes numbers
- The geometry of meaning: vectors in embedding space
- How models learn through training
- Understanding parameters, dimensions, and the Transformer architecture
- Why LLM output is non-deterministic and how to control it

### 3. [AI Model Capabilities](./learn/models.md)
**Learn how to understand the differences in AI models' capabilities**

- Model size and capacity considerations
- Context windows and output limits
- Reasoning controls and multimodality
- Tool calling and structured output
- Safety and control features
- Cost and latency analysis
- Practical evaluation protocols

### 4. [Prompt Engineering Resources](./learn/prompt.md)
**Essential resources for effective prompt engineering**

- Links to official guides from Claude and OpenAI
- Tools for prompt optimization
- Best practices for context engineering

## 🚀 Getting Started

1. **Clone this repository:**
   ```bash
   git clone https://github.com/marcopesani/vibeworking-bootstrap.git
   cd vibeworking-bootstrap
   ```

## 🛠️ Repository Structure

```
vibeworking-bootstrap/
├── learn/                    # Core learning materials
│   ├── denoise.md           # Image generation and denoising
│   ├── llm.md               # Large language models
│   ├── models.md            # Model capabilities and selection
│   └── prompt.md            # Prompt engineering resources
├── docs/                    # Additional documentation (empty)
├── .cursor/                 # Cursor editor configuration
│   ├── mcp.json            # MCP server configuration
│   └── rules/               # Custom workflow rules
│       └── figma-workflow.mdc # Figma design-to-code workflow
├── package.json            # Node.js project configuration
├── LICENSE                  # Apache 2.0 License
└── README.md               # This file
```

## ⚙️ Cursor Editor Configuration

This repository includes baseline configuration for the Cursor editor to enhance the development experience:

### MCP Servers (`.cursor/mcp.json`)
- **Figma Desktop**: Connects to local Figma instance for design-to-code workflows
- **Chrome DevTools**: Enables browser automation and testing capabilities

### Custom Workflow Rules (`.cursor/rules/`)
- **Figma Workflow** (`figma-workflow.mdc`): Step-by-step process for converting Figma designs to code, including:
  - Design analysis and documentation
  - Component structure mapping
  - Design token extraction (spacing, typography, colors)
  - Semantic structure documentation

This configuration makes the repository ready for design-to-code workflows and enhanced AI-assisted development.

## 📄 License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.

## 🤝 Contributing

This is a bootstrap template designed to be customized for your specific workshop needs. Feel free to:

- Fork the repository for your own workshop
- Modify content to match your audience
- Add your own examples and case studies
- Extend the materials with additional topics

## 📞 Contact

**Author:** Marco Pesani  
**Email:** marco.pesani@gmail.com  
**Repository:** [https://github.com/marcopesani/vibeworking-bootstrap](https://github.com/marcopesani/vibeworking-bootstrap)
