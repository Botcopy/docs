# Data Sources

Data Store is Dialogflow's built-in feature that allows your agent to search and retrieve information from knowledge bases to answer user queries. When configured, your bot can automatically pull relevant content from websites, documents, and other sources to provide accurate, context-aware responses with proper attribution.

## What is Data Store?

Data Store enables your Dialogflow agent to:

- **Search Knowledge Bases**: Query content from websites, structured data, or unstructured documents
- **Generate Responses**: Use retrieved information to answer user questions
- **Provide Citations**: Automatically include source references with bot responses
- **Stay Up-to-Date**: Pull current information from live sources without manual training

This feature is particularly useful for:
- FAQ responses from existing documentation
- Product information from catalogs
- Support content from help centers
- Information retrieval from large document repositories

## Setting Up Data Store

### Prerequisites

- A Dialogflow CX or ES agent
- Access to the Google Cloud Console
- Source content (websites, documents, or structured data)

### Configuration Steps

1. **Navigate to Data Store Settings**
   - Open your Dialogflow agent in the Google Cloud Console
   - Go to the "Agent Assist" or "Generative AI" section
   - Select "Data Store" or "Search"

2. **Create a Data Store**
   - Click "Create Data Store" or "Add Data Store"
   - Choose your data source type:
     - **Website**: Crawl public web pages
     - **Structured Data**: Upload CSV or JSON files
     - **Unstructured Documents**: Upload PDFs, text files, or other documents

3. **Configure Your Data Source**

   **For Website Data:**
   - Enter the URLs you want to crawl
   - Configure crawl depth and scope
   - Set update frequency for keeping content current

   **For Document Data:**
   - Upload your files (PDF, TXT, DOCX, etc.)
   - Organize documents with metadata
   - Define indexing preferences

   **For Structured Data:**
   - Upload CSV or JSON files
   - Map fields to searchable attributes
   - Define schema for structured queries

4. **Enable Citations**
   - In your Data Store settings, ensure "Return citations" is enabled
   - This allows the bot to include source references with responses
   - Configure citation format preferences

5. **Connect to Your Agent**
   - Link the Data Store to your Dialogflow agent
   - Configure when the agent should query the Data Store (e.g., for specific intents or as a fallback)
   - Set confidence thresholds for using Data Store responses

6. **Test Your Configuration**
   - Use the Dialogflow simulator to test queries
   - Verify that responses include relevant information from your Data Store
   - Check that citations appear correctly

## Data Store Best Practices

### Content Organization

- **Keep Content Updated**: Regularly refresh your Data Store with current information
- **Use Clear Titles**: Ensure documents and pages have descriptive titles for better citations
- **Structure Information**: Organize content logically to improve retrieval accuracy
- **Remove Duplicates**: Eliminate redundant content to avoid confusion

### Optimization

- **Monitor Performance**: Review which queries successfully retrieve information
- **Refine Sources**: Add or remove content sources based on usage patterns
- **Test Edge Cases**: Verify responses for uncommon or complex queries
- **Update Regularly**: Keep website crawls and documents synchronized with source changes

### Citation Quality

- **Verify Links**: Ensure URLs in citations are accessible to end users
- **Provide Context**: Use meaningful titles and subtitles for citations
- **Check Accuracy**: Review that citations match the content used in responses

## Citations

Citations display source references for bot responses generated from Data Store queries. When enabled, citations provide users with transparent attribution to the knowledge sources the bot used to generate its answers.

### When Citations Render

Citations appear in the chat when Dialogflow returns citation data from Data Store queries. This typically occurs when:

- Your agent is configured to use Data Store as a knowledge source
- The bot's response is generated from content found in your Data Store
- Dialogflow includes citation metadata in the response payload

Citations help users understand where the bot's information comes from and allow them to access the original source for more details.

### Icon Display Logic

Citations display different icons to indicate the type of source:

- **Website Icon**: Displays when the citation includes a clickable link to a web source
- **Document Icon**: Displays when the citation references a document without a direct link

This distinction helps users quickly understand whether they can click through to view the source or if the citation is for reference only.

### User Interaction

Citations behave differently based on whether they include an actionable link:

#### Citations with Links

When a citation includes a web link:
- Users can click to open the source
- External URLs open in a new browser tab
- Same-domain URLs open in the parent frame
- The link provides direct access to the referenced content

#### Special Behavior for Local Anchors

When a citation links to an anchor on the current page (e.g., `#section-name`), clicking the citation will close the chat widget and navigate to that section on the page. This allows users to seamlessly access relevant content on your website.

### Citation Data

Each citation provides:
- **Title**: The name or title of the source
- **Subtitle** (optional): Additional context about the source
- **Action Link** (optional): URL to the source material

### Language Support

Citations support both left-to-right (LTR) and right-to-left (RTL) text directions, automatically adjusting layout and alignment based on the user's selected language.