# 00_Context

Navigation:
- [..](../)
- [01_Semantic_Docs](01_Semantic_Docs.md)
- [02_Automated_Frontend_Semantic_Works](02_Automated_Frontend_Semantic_Works.md)
- [03_SIDEQUEST_Diviocheck.py](03_SIDEQUEST_Diviocheck.py.md)
- [04_Divio_Docs_Gen](04_Divio_Docs_Gen.md)
- [05_Repo_Collector_And_App_Mu_Info](05_Repo_Collector_And_App_Mu_Info.md)
- [06_SIDEQUEST_AnyArgs](06_SIDEQUEST_AnyArgs.md)
- [07_Current_Implementation](07_Current_Implementation.md)

The following table is an overview of the original state, and the subsequent iterations upon it.

<table>
    <tr>
        <th>Method</th>
        <th>Keeps the semantic.works style</th>
        <th>Automatic adding, categorising and titling of a project to the website</th>
        <th>URL change after selecting a project</th>
        <th>Parsed documentation</th>
        <th>Versioned documentation</th>
        <th>Links to the Docker images</th>
        <th>Links to the (GitHub) repositories</th>
        <th>Minimal vendor-lock-in</th>
        <th>(Fully) responsive</th>
        <th>Accessibility improvements</th>
        <th>Dogfooding</th>
    </tr>
    <tr>
        <th>Old semantic.works website</th>
        <th>✔</th>
        <th>❌</th>
        <th>❌</th>
        <th>❌</th>
        <th>❌</th>
        <th>❌</th>
        <th>❌</th>
        <th>❌</th>
        <th>❌</th>
        <th>❌</th>
        <th>❌</th>
    </tr>
    <tr>
        <th>Semantic-docs</th>
        <th>❌</th>
        <th>✔</th>
        <th>✔</th>
        <th>❌</th>
        <th>❌</th>
        <th>❌</th>
        <th>❌</th>
        <th>✔</th>
        <th>❕</th>
        <th>❌</th>
        <th>❌</th>
    </tr>
    <tr>
        <th>Automated frontend Semantic.works</th>
        <th>✔</th>
        <th>✔</th>
        <th>✔</th>
        <th>❌</th>
        <th>❌</th>
        <th>❌</th>
        <th>❌</th>
        <th>✔</th>
        <th>❌</th>
        <th>❌</th>
        <th>❌</th>
    </tr>
    <tr>
        <th>Divio-Docs-Gen</th>
        <th>❌</th>
        <th>✔</th>
        <th>✔</th>
        <th>✔</th>
        <th>❌</th>
        <th>❌</th>
        <th>❌</th>
        <th>✔</th>
        <th>❕</th>
        <th>❌</th>
        <th>❌</th>
    </tr>
    <tr>
        <th>Repo-Collector, app-mu-info-archive</th>
        <th>N/A</th>
        <th>✔</th>
        <th>N/A</th>
        <th>❌</th>
        <th>❌</th>
        <th>✔</th>
        <th>✔</th>
        <th>✔</th>
        <th>N/A</th>
        <th>N/A</th>
        <th>❌</th>
    </tr>
    <tr>
        <th>{App-mu-info, Semantic.works}-rework, Divio Docs Parser</th>
        <th>✔</th>
        <th>✔</th>
        <th>✔</th>
        <th>✔</th>
        <th>✔</th>
        <th>✔</th>
        <th>✔</th>
        <th>✔</th>
        <th>✔</th>
        <th>✔</th>
        <th>✔</th>
    </tr>
</table>

✔: Implemented
❕: Implemented with caveats
❌: Not implemented



This is what the following however-many-documents in this section tries to fix.
