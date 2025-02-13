# Latest Causal Inference Research

This page displays a regularly updated list of the newest research papers about causal inference from arXiv. The list is automatically refreshed daily at midnight.

<style>
    body {
        font-family: Arial, sans-serif;
        margin: 20px;
        background: #f5f5f5;
    }
    table {
        width: 100%;
        border-collapse: collapse;
        background: white;
        box-shadow: 0 1px 3px rgba(0,0,0,0.2);
    }
    th, td {
        padding: 12px;
        text-align: left;
        border-bottom: 1px solid #ddd;
    }
    th {
        background-color: #4CAF50;
        color: white;
    }
    tr:hover {
        background-color: #f5f5f5;
    }
    .last-updated {
        margin-bottom: 20px;
        color: #666;
    }
    .loading {
        text-align: center;
        padding: 20px;
        font-style: italic;
        color: #666;
    }
</style>

<div class="last-updated">Last updated: <span id="update-time"></span></div>
<div id="loading" class="loading">Loading latest papers...</div>
<table id="papers-table">
    <thead>
        <tr>
            <th>Title</th>
            <th>Authors</th>
            <th>Published</th>
            <th>Link</th>
        </tr>
    </thead>
    <tbody id="papers-body">
    </tbody>
</table>

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
- Displays paper titles, authors, and publication dates
- Focused specifically on causal inference research
