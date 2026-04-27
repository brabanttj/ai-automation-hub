# AI Capability Finder - Enterprise Tool Tracker

## Overview

The AI Capability Finder helps track and discover AI tools available within your enterprise, mapped to specific business capabilities. This solves the common problem of AI tool sprawl where teams adopt tools faster than governance can track them.

## Key Improvements in V2

### 🎯 Enhanced Functionality
- **Multi-dimensional filtering**: Filter by category, platform, status, and search term simultaneously
- **Status-based filtering**: Find all enabled, not enabled, or unsupported capabilities
- **CSV Export**: Export filtered results for reporting and sharing
- **Comprehensive detail view**: Click any capability to see all tools that support it
- **Separated data layer**: Easy to maintain and update without touching UI code

### 🔧 Technical Improvements
- **Modular architecture**: Data separated from UI components
- **Better performance**: Optimized filtering with useMemo
- **Maintainable styles**: All styles in one object for easy theming
- **No external dependencies**: Pure React, no animation libraries
- **Flexible data structure**: Easy to add new platforms or statuses

## File Structure

```
├── lt_ai_capability_finder_v2.jsx   # Main React component
├── ai-capabilities-data.js          # Data layer (easy to update)
└── AI_CAPABILITY_FINDER_README.md   # This file
```

## How to Use

### For End Users

1. **Search**: Type in the search box to find capabilities by name, category, or tool
2. **Filter by Category**: Use the category dropdown to focus on specific business areas
3. **Filter by Platform**: Select a platform (Excel, Teams, etc.) to see what's available
4. **Filter by Status**: Show only enabled tools, not enabled, or unsupported
5. **View Details**: Click any capability card to see full platform support matrix
6. **Export**: Click "Export to CSV" to download current filtered view

### For Administrators

#### Adding a New Tool/Platform

1. Open `ai-capabilities-data.js`
2. Add the new platform to the `PLATFORMS` array:
```javascript
export const PLATFORMS = ["Excel", "Outlook", "Word", "Teams", "SharePoint", "Azure", "Jira", "PowerBI"];
```
3. No other code changes needed - the UI will automatically render the new column

#### Adding Documentation Links

1. Add the link to the `DOCS` object:
```javascript
export const DOCS = {
  copilotPowerBI: "https://intranet.lendingtree.com/docs/copilot-powerbi",
  // ...
};
```

#### Adding a New Category

1. Add to the `CATEGORY_META` object:
```javascript
export const CATEGORY_META = {
  "Sales & CRM": {
    icon: "📈",
    color: "#10b981",
    description: "Manage sales pipeline and customer relationships"
  },
  // ...
};
```

#### Adding/Updating Capabilities

1. Find the appropriate category section in `MATRIX_ROWS`
2. Add a new entry:

```javascript
{
  category: "Data",
  subCategory: "Machine Learning",
  capability: "Train & deploy ML models",
  platform: "Azure ML",
  cells: {
    ...blank(),  // Start with all blank
    Azure: ps(STATUS.ENABLED, DOCS.azureML),
    Excel: ps(STATUS.NOT_SUPPORTED),
    // ... continue for all platforms
  }
}
```

**Status Options:**
- `STATUS.ENABLED` - Tool is available and approved (provide link if possible)
- `STATUS.NOT_ENABLED` - Tool exists but not enabled for users
- `STATUS.NOT_SUPPORTED` - Capability doesn't make sense for this platform
- `STATUS.BLANK` - No information populated yet

#### Marking a Capability as "Not Populated"

Simply omit the `platform` field or set it to empty string:

```javascript
{
  category: "Product Management",
  subCategory: "Planning",
  capability: "Build roadmaps",
  platform: "",  // or omit entirely
  cells: blank(),  // All platforms blank
}
```

## Data Model

### Row Structure
```javascript
{
  category: string,           // "Data", "Engineering & Code", etc.
  subCategory: string,        // More specific grouping
  capability: string,         // What the tool helps you do
  platform: string,           // Tool name: "Microsoft Copilot", "ChatGPT Enterprise"
  cells: {
    [platform]: {
      status: STATUS,         // Enabled, Not Enabled, Not Supported, Blank
      link: string           // Documentation URL (optional)
    }
  }
}
```

### Multiple Tools Per Capability

The system supports multiple tools for the same capability:

```javascript
// Example: Multiple tools can generate code
makeRow("Engineering & Code", "Code Generation", "Generate code", "GitHub Copilot", { ... }),
makeRow("Engineering & Code", "Code Generation", "Generate code", "Amazon CodeWhisperer", { ... }),
makeRow("Engineering & Code", "Code Generation", "Generate code", "ChatGPT Enterprise", { ... }),
```

The UI will show all three as separate rows in the detail panel.

## Maintenance Workflow

### Monthly Review Process

1. **Audit new tools**: Check for shadow IT or newly approved tools
2. **Update statuses**: Tools move from pilot → enabled or deprecated
3. **Add documentation links**: As internal docs are created
4. **Export snapshot**: Create CSV backup of current state

### Governance Integration

Use this tool to:
- **Identify redundancy**: Filter by capability to see overlapping tools
- **Track adoption**: Count "Enabled" vs "Not Enabled" by category
- **Gap analysis**: Find blank capabilities that need tool coverage
- **Budget planning**: Export enabled tools for license renewal planning

## Common Patterns

### Finding Tool Overlap
1. Search for a capability (e.g., "draft documents")
2. Click the capability
3. Review how many different tools support it
4. Consider consolidation if >3 tools

### Checking Platform Coverage
1. Filter by platform (e.g., "Teams")
2. See all capabilities available in that platform
3. Identify gaps where Teams could be leveraged more

### Building Training Plans
1. Filter by Status = "Enabled"
2. Export to CSV
3. Group by category to build training curriculum

## Future Enhancements

Possible additions:
- **User role mapping**: Show which roles use which capabilities
- **Usage metrics**: Integrate actual usage data from tools
- **Cost tracking**: Add license cost per tool
- **Integration API**: Pull data from tool APIs automatically
- **Approval workflow**: Built-in process for requesting new tools

## Support

For questions or to request updates to the matrix, contact:
- **Tool Owner**: [Your Team]
- **Data Updates**: Edit `ai-capabilities-data.js` and submit PR
- **Bug Reports**: [Your Issue Tracker]
