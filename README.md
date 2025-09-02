# JSL Secrets Management - Research & Development

Enterprise-grade secure credential management patterns for JMP Statistical Language (JSL) scripts, demonstrating best practices for handling sensitive authentication data in analytical workflows.

## Overview

This repository contains two comprehensive examples of secure credential management in JSL environments:

### 🔐 [Box Drive Example](./Box%20Drive%20Example/)
Secure Box API integration with JWT authentication, recursive file discovery, and enterprise-grade credential management.

**Key Features:**
- Encrypted JSL credential storage with zero-exposure authentication
- Recursive file search and download from Box cloud storage
- Complete workflow: authenticate → search → download → concatenate

**📖 [Full Documentation](./Box%20Drive%20Example/docs/box_usage.md)**

### 🗄️ [Snowflake Example](./Snowflake%20Example/)
Secure database connection management with three-tier credential protection for Snowflake data warehouse integration.

**Key Features:**
- Template → Raw → Encrypted credential file workflow
- Gatekeeper functions with key-based access control
- Secure SQL query execution with automatic credential cleanup
- Production-ready database connection patterns

**📖 [Full Documentation](./Snowflake%20Example/docs/snowflake_usage.md)**

## Security Architecture

Both examples implement enterprise security patterns based on these core principles:

### 🔒 JSL Encryption & Credential Storage
- **Encrypted JSL Files**: Use JMP's built-in encryption (`//-e12.1` header) for production credentials
- **Three-Tier Approach**: Template → Raw (gitignored) → Encrypted (production)
- **Never Commit Raw Credentials**: Always use `.gitignore` for files containing actual secrets
- **File Permissions**: Protect local credential files with appropriate OS-level permissions

### 🔑 Access Control Patterns
- **Gatekeeper Functions**: `get_secrets(key)` requires authentication key before returning credentials
- **Key-Based Authentication**: Each credential access requires proper authorization key
- **Individual Getter Functions**: Separate functions for each credential component
- **Fail-Safe Validation**: Multiple layers of access control with secure error handling

### 🧹 Memory Safety & Cleanup
- **Automatic Cleanup**: Credentials cleared from memory immediately after use
- **Minimal Exposure Window**: Credentials only exist in memory during active operations
- **Error-Safe Clearing**: Memory cleanup occurs even when operations fail
- **Zero-Parameter Security**: Authentication functions never expose credentials in parameters

### 🛡️ Production Security Practices
- **Credential Rotation**: Regular password and certificate updates with encrypted file refresh
- **Minimal Privilege**: Database accounts and API keys with only required permissions
- **Environment Separation**: Different credential sets for development, staging, production
- **Audit Trail**: Logging authentication events without exposing sensitive data
- **Network Security**: HTTPS/TLS for all API communications, firewall considerations

### 📋 Implementation Checklist
- ✅ Create template files with placeholder credentials (safe to commit)
- ✅ Generate raw files with actual credentials (add to `.gitignore`)
- ✅ Encrypt raw files for production storage (safe to commit encrypted version)
- ✅ Implement gatekeeper functions with unique access keys
- ✅ Add automatic memory cleanup to all credential-handling functions
- ✅ Test credential rotation procedures
- ✅ Verify no credentials appear in logs or error messages

## Quick Start

1. **Choose your integration**: Box API or Snowflake database
2. **Follow the setup guide** in the respective documentation
3. **Configure your credentials** using the secure patterns provided
4. **Run the examples** to see secure credential management in action

## Repository Structure

```
RnD_JSL_Secrets_Management/
├── README.md                          # This file
├── secrets_template.jsl               # General credential template
├── Box Drive Example/
│   ├── box_connection_example.jsl     # Main JSL implementation
│   ├── box_connection_encrypted.jsl   # Encrypted credentials
│   ├── box_connection_template.jsl    # Template file
│   └── docs/
│       └── box_usage.md              # Complete Box integration guide
└── Snowflake Example/
    ├── snowflake_connection_*.jsl     # Database connection files
    └── docs/
        └── snowflake_usage.md        # Complete Snowflake integration guide
```

## Conference Presentation

This entire repository was presented at **JMP Discovery 2025**:
**[The Secret is Out! Oh Wait, No It's Not](https://community.jmp.com/t5/Abstracts/The-Secret-is-Out-Oh-Wait-No-It-s-Not-2025-US-30MP-2510/ev-p/884381?profile.language=en)** - Demonstrating secure credential management patterns in analytical workflows across multiple integration examples.

## Getting Started

1. **Read the documentation** for your chosen integration
2. **Set up your credentials** following the security patterns
3. **Test the examples** in a development environment
4. **Adapt the patterns** for your specific use case

## Security Best Practices

- ✅ Never commit raw credentials to version control
- ✅ Always use encrypted JSL files for production
- ✅ Implement key-based access control
- ✅ Clear credentials from memory after use
- ✅ Use minimal-privilege accounts
- ✅ Rotate credentials regularly

## Support

This is a research and development project demonstrating secure credential management patterns in JSL. For questions about implementation or security practices, consult your IT security team.

---

**Note:** This repository contains defensive security tools and educational examples only. All credential examples use placeholder values and require proper configuration for production use.