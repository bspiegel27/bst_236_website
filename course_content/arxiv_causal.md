---
layout: default
title: Latest Causal Inference Research
---

<div style="text-align: right; margin: 10px;">
    <a href="https://bspiegel27.github.io/bst_236_website/" style="text-decoration: none; color: #0366d6; font-weight: bold;">← Home</a>
</div>

# Latest Causal Inference Research

This page displays a regularly updated list of the newest research papers about causal inference from arXiv. The list is automatically refreshed daily at midnight.

<div class="last-updated">Last updated: <span id="update-time"></span></div>
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
            // Update path to be relative to GitHub Pages root
            const response = await fetch('https://bspiegel27.github.io/bst_236_website/data/arxiv-cache.xml');
            const text = await response.text();
            
            // Get last update time from XML comment
            const updateMatch = text.match(/<!-- Last updated: (.*?) -->/);
            const updateTime = updateMatch ? updateMatch[1] : new Date().toISOString();
            document.getElementById('update-time').textContent = new Date(updateTime).toLocaleString();
            
            const parser = new DOMParser();
            const xml = parser.parseFromString(text, 'text/xml');
            const ns = 'http://www.w3.org/2005/Atom';  // ArXiv uses Atom feed format
            
            // Extract papers using correct namespace
            return Array.from(xml.getElementsByTagNameNS(ns, 'entry')).map(entry => ({
                title: entry.getElementsByTagNameNS(ns, 'title')[0].textContent.trim(),
                authors: Array.from(entry.getElementsByTagNameNS(ns, 'author'))
                    .map(author => author.getElementsByTagNameNS(ns, 'name')[0].textContent.trim())
                    .join(', '),
                abstract: entry.getElementsByTagNameNS(ns, 'summary')[0].textContent.trim(),
                published: new Date(entry.getElementsByTagNameNS(ns, 'published')[0].textContent)
                    .toLocaleDateString(),
                link: entry.getElementsByTagNameNS(ns, 'id')[0].textContent
            }));
        } catch (error) {
            console.error('Error fetching papers:', error);
            document.getElementById('loading').textContent = 'Error loading papers. Please try again later.';
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
