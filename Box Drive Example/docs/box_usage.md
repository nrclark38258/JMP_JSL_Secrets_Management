# JSL Secrets Management for Box API Integration

Secure Box API integration demonstrating the [enterprise security patterns](../../README.md#security-architecture) defined in the main repository documentation.

## Overview

This example implements JWT-based authentication with Box Platform Applications for secure file discovery, download, and data consolidation workflows.

**Key Features:**
- Encrypted credential management with zero-exposure authentication
- Recursive file search with configurable depth and filtering
- Complete workflow: authenticate → search → download → concatenate

## File Structure

```
Box Drive Example/
├── box_connection_example.jsl           # Main JSL implementation
├── box_connection_encrypted.jsl         # Encrypted credentials (production)
├── box_connection_template.jsl          # Template with placeholder credentials
└── docs/
    └── box_usage.md                     # This documentation
```

## Core Functions

### 1. `get_box_access_token()`
Secure authentication function that generates Box API access tokens using JWT.

**Parameters:** None (zero-parameter security)

**Returns:**
- Valid Box API access token

**Example:**
```jsl
token = get_box_access_token();
// Returns: "Bearer eyJ0eXAiOiJKV1QiLCJh..."
```

### 2. `recursive_search_files_jsl(folder_id, file_suffix, max_depth)`
Recursively searches Box folders for files matching specified criteria.

**Parameters:**
- `folder_id` - Starting Box folder ID
- `file_suffix` - File extension to search for (e.g., ".jmp")
- `max_depth` - Maximum recursion depth

**Returns:**
- Array of file metadata objects

**Example:**
```jsl
files = recursive_search_files_jsl("332509763387", ".jmp", 5);
```

### 3. `download_box_files(found_files, download_folder)`
Downloads discovered files to local storage with progress tracking.

**Parameters:**
- `found_files` - Array from recursive search
- `download_folder` - Local destination path

**Returns:**
- Download results summary

**Example:**
```jsl
result = download_box_files(files, "Test Files/");
```

## Quick Setup

### 1. Box Platform Application
1. Create Box Platform Application at [developer.box.com](https://developer.box.com/)
2. Configure for Server Authentication (JWT)
3. Download configuration file and authorize in Box Admin Console

### 2. Configure Credentials
1. Copy `box_connection_template.jsl` to create your credential file
2. Replace placeholder values with actual Box application credentials
3. Choose deployment method:
   - **Development**: Use raw file (add to `.gitignore`)
   - **Production**: Encrypt file using JMP's encryption feature

### 3. Basic Usage
```jsl
Names default to here(1);
include("box_connection_encrypted.jsl");

// Configure search parameters
starting_folder_id = "your_folder_id";
file_suffix_to_search = ".jmp";
max_search_depth = 5;

// Run complete workflow
main_box_connection();
```

## Sample Output

```
Successfully authenticated as: John Doe (ID: 12345)

============================================================
Starting recursive file search...
Searching for files ending with '.jmp'
Starting from folder ID: 332509763387
============================================================
Searching in folder: Analytics Projects (ID: 332509763387)
  📁 Folder: Reports (ID: 445566778899)
    ✓ FOUND: analysis.jmp (ID: 123456789)
============================================================
Search completed! Found 1 files with suffix '.jmp'
```

## Security Implementation

This Box integration implements the [enterprise security patterns](../../README.md#security-architecture) defined in the main repository documentation.

### Box-Specific Security Features
- **JWT-Based Authentication**: Secure token generation using encrypted private keys
- **Box Enterprise Integration**: Supports Box Platform Applications with Server Authentication
- **Automatic Token Refresh**: Handles token expiration gracefully
- **API Rate Limiting**: Built-in handling for Box API rate limits

## Requirements

### JSL Version
- JMP with Python integration
- Python packages: `PyJWT`, `cryptography`
- Encrypted `box_connection_encrypted.jsl` file

## Configuration

Both implementations search for `.jmp` files starting from folder ID `332509763387` with a maximum depth of 5 levels. Modify these parameters in the respective files to customize your search.

## Box API Endpoints

- `POST /oauth2/token` - JWT authentication
- `GET /users/me` - User verification
- `GET /folders/{id}` - Folder metadata
- `GET /folders/{id}/items` - Folder contents

## Security Notes

See the [main security documentation](../../README.md#security-architecture) for comprehensive security practices. Box-specific considerations:

- Regularly rotate JWT certificates in Box Developer Console
- Monitor Box Admin Console for application authorization status
- Use Box Enterprise accounts with appropriate user permissions

## Process Workflow

The Box integration follows this simplified workflow:

1. **Authentication**: Secure JWT-based authentication using encrypted credentials
2. **File Discovery**: Recursive search through Box folder structure with configurable depth
3. **File Download**: Batch download of matching files to local storage
4. **Data Concatenation**: Merge all downloaded JMP files into unified dataset

### Configuration Constants

| Constant | Default Value | Purpose |
|----------|---------------|---------|
| `DEFAULT_FOLDER_ID` | "332509763387" | Box folder to search |
| `DEFAULT_FILE_SUFFIX` | ".jmp" | File extension filter |
| `DEFAULT_MAX_DEPTH` | 5 | Recursion limit |
| `DEFAULT_DOWNLOAD_FOLDER` | "Test Files/" | Local download path |
| `DEFAULT_CONCAT_FILE` | "Concatenated_Files.jmp" | Output file name |