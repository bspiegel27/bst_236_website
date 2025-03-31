---
layout: default
title: Latest Causal Inference Research
---

<div style="text-align: right; margin: 10px;">
    <a href="https://bspiegel27.github.io/bst_236_website/" style="text-decoration: none; color: #0366d6; font-weight: bold;">← Home</a>
</div>

# Latest Causal Inference Research

This page displays a regularly updated list of the newest research papers about causal inference from arXiv. The list is automatically refreshed daily at midnight.

<div class="last-updated">Last updated: <span id="update-time">2025-03-31 01:10:45 UTC</span></div>
<div id="loading">Loading latest papers...</div>
<table id="papers-table">
    <thead>
        <tr>
            <th>Title</th>
            <th>Authors</th>
            <th>Abstract</th>
            <th>Published</th>
            <th>Link</th>
        </tr>
    </thead>
    <tbody id="papers-body">
    </tbody>
</table>

<style>
table {
    width: 200%;
    margin-left: -50%;
    margin-right: -50%;
    table-layout: fixed;
}

/* Column widths */
th:nth-child(1), td:nth-child(1) { width: 15%; }  /* Title */
th:nth-child(2), td:nth-child(2) { width: 12%; }  /* Authors */
th:nth-child(3), td:nth-child(3) { width: 60%; }  /* Abstract */
th:nth-child(4), td:nth-child(4) { width: 8%; }   /* Published */
th:nth-child(5), td:nth-child(5) { width: 5%; }   /* Link */
</style>

<script>
    async function fetchPapers() {
        try {
            // Direct ArXiv API query
            const today = new Date();
            const yesterday = new Date(today);
            yesterday.setDate(yesterday.getDate() - 1);
            
            const query = `https://export.arxiv.org/api/query?search_query=all:causal+inference&sortBy=submittedDate&sortOrder=descending&start=0&max_results=10`;
            const response = await fetch(query);
            if (!response.ok) {
                throw new Error(`HTTP error! status: ${response.status}`);
            }
            const text = await response.text();
            
            // Update timestamp
            document.getElementById('update-time').textContent = new Date().toLocaleString();
            
            // Parse XML response
            const parser = new DOMParser();
            const xml = parser.parseFromString(text, 'application/xml');
            
            // Check for parsing errors
            const parserError = xml.querySelector('parsererror');
            if (parserError) {
                throw new Error('XML parsing error: ' + parserError.textContent);
            }

            // Extract papers
            return Array.from(xml.getElementsByTagName('entry')).map(entry => ({
                title: entry.querySelector('title')?.textContent?.trim() || 'No title',
                authors: Array.from(entry.getElementsByTagName('author'))
                    .map(author => author.querySelector('name')?.textContent?.trim())
                    .filter(Boolean)
                    .join(', ') || 'No authors',
                abstract: entry.querySelector('summary')?.textContent?.trim() || 'No abstract',
                published: entry.querySelector('published') ? 
                    new Date(entry.querySelector('published').textContent).toLocaleDateString() :
                    'No date',
                link: Array.from(entry.getElementsByTagName('link'))
                    .find(link => link.getAttribute('title') === 'pdf')
                    ?.getAttribute('href') || entry.querySelector('id')?.textContent || '#'
            }));
        } catch (error) {
            console.error('Error fetching papers:', error);
            document.getElementById('loading').textContent = `Error loading papers: ${error.message}`;
            return [];
        }
    }

    function updateTable(papers) {
        const tbody = document.getElementById('papers-body');
        tbody.innerHTML = papers.map(paper => `
            <tr>
                <td>${paper.title}</td>
                <td>${paper.authors}</td>
                <td>${paper.abstract}</td>
                <td>${paper.published}</td>
                <td><a href="${paper.link}" target="_blank">View</a></td>
            </tr>
        `).join('');
        document.getElementById('loading').style.display = 'none';
    }

    async function updatePapers() {
        const papers = await fetchPapers();
        updateTable(papers);
    }

    // Only fetch once when page loads
    updatePapers();
</script>

## Features
- Daily updates at midnight
- Direct links to arXiv papers
- Displays paper titles, authors, abstracts, and publication dates- Focused specifically on causal inference research
