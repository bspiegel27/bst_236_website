---
layout: default
title: Latest Causal Inference Research
---

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
    width: 100%;
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
        const query = 'search_query=all:causal+inference&submittedDate:[${yesterday}+TO+${today}]&sortBy=submittedDate&sortOrder=descending&start=0&max_results=10';
        const response = await fetch(`https://export.arxiv.org/api/query?${query}`);
        const text = await response.text();
        const parser = new DOMParser();
        const xml = parser.parseFromString(text, 'text/xml');
        return Array.from(xml.getElementsByTagName('entry')).map(entry => ({
            title: entry.querySelector('title').textContent,
            authors: Array.from(entry.getElementsByTagName('author'))
                .map(author => author.textContent)
                .join(', '),
            abstract: entry.querySelector('summary').textContent,
            published: new Date(entry.querySelector('published').textContent)
                .toLocaleDateString(),
            link: entry.querySelector('id').textContent
        }));
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
        document.getElementById('update-time').textContent = 
            new Date().toLocaleString();
    }

    async function updatePapers() {
        try {
            const papers = await fetchPapers();
            updateTable(papers);
        } catch (error) {
            console.error('Error fetching papers:', error);
            document.getElementById('loading').textContent = 
                'Error loading papers. Please try again later.';
        }
    }

    // Update immediately when page loads
    updatePapers();

    // Calculate time until next midnight
    const now = new Date();
    const tomorrow = new Date(now);
    tomorrow.setDate(tomorrow.getDate() + 1);
    tomorrow.setHours(0, 0, 0, 0);
    const timeUntilMidnight = tomorrow - now;

    // Set up daily updates at midnight
    setTimeout(() => {
        updatePapers();
        setInterval(updatePapers, 24 * 60 * 60 * 1000);
    }, timeUntilMidnight);
</script>

## Features
- Daily updates at midnight
- Direct links to arXiv papers
- Displays paper titles, authors, abstracts, and publication dates
- Focused specifically on causal inference research
