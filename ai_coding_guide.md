# How to Use AI Coding Assistants for Bioinformatics

In modern bioinformatics, AI coding assistants like **Google Antigravity** (IDE-integrated agent), **ChatGPT**, and **Google Gemini** are powerful tools to accelerate your analysis. This guide explains how to effectively use these tools for your single-cell analysis projects.

---

## 1. Choosing the Right Tool

| Tool | Best Used For |
| :--- | :--- |
| **Google Antigravity (IDE Agent)** | **Context-aware editing.** It can read your open files, understand your project structure, and directly edit code within your IDE (e.g., VS Code). Ideal for complex refactoring or adding new features to existing scripts. |
| **ChatGPT (OpenAI)** | **General coding questions & debugging.** Excellent at explaining error messages, generating small code snippets from scratch, and explaining biological concepts. |
| **Google Gemini** | **Multimodal understanding & large context.** Great for processing large amounts of text/code context and integrating with other Google services. |

---

## 2. Best Practices for Prompt Engineering

To get the best results, treat the AI as a junior bioinformatician who needs clear instructions.

### A. Be Specific and Provide Context
Instead of saying *"Fix this code"*, explain *what* the data looks like.

**Bad Prompt:**
> "My Seurat clustering isn't working. Fix it."

**Good Prompt:**
> "I am analyzing a Seurat object named `syno.rna.jia` with 20,000 cells. When I run `FindClusters(resolution=0.5)`, I get an error saying 'neighbor graph not found'. I previously ran `RunPCA` and `RunHarmony`. Here is my code snippet..."

### B. Mention the Libraries and Versions
Bioinformatics packages update frequently.
> "I am using **Seurat v5** in R. How do I access the count matrix?"

### C. Ask for Explanations
> "I don't understand what `RunHarmony(group.by.vars = 'sample')` does. Can you explain the `group.by.vars` parameter in simple terms?"

---

## 3. Example Workflows

### Scenario 1: Debugging an Error
**You:**
> "I got this error when running `RunUMAP`:
> `Error: dim 1 is greater than the number of available PCs`
> What does this mean and how do I fix it?"

**AI:**
> "This error means you are trying to use more Principal Components (PCs) than you calculated. Check your `RunPCA` step or reduce the `dims` parameter in `RunUMAP`..."

### Scenario 2: Generating Visualization Code
**You:**
> "I have a Seurat object `t_cells` with a column `cell_type`.
> Please write R code to create a DotPlot showing the expression of 'CD3E', 'CD4', 'CD8A', and 'FOXP3', grouped by `cell_type`. Rotate the x-axis labels vertically."

### Scenario 3: Refactoring / improving code
**You:**
> "I have a loop that subsets the data 5 times. Is there a more efficient way to do this using `lapply` or a function in R?"

---

## 4. Using Google Antigravity (IDE Agent)

For tools integrated directly into your editor (like Antigravity):

1.  **Open relevant files:** Open the Rmd file and the data file location (in file explorer) so the agent can "see" them.
2.  **Highlight code:** If you have a question about a specific chunk, highlight it before asking.
3.  **Iterate:** If the agent makes a mistake, tell it: *"Note that I am using Seurat v5, so we need to use `Seurat::LayerData` instead of `GetAssayData`."*

---

## 5. Important Cautions ⚠️

1.  **Hallucination:** AI can confidently invent functions that do not exist (e.g., `Seurat::FindAllTheBestMarkers()`). **Always run the code to verify it.**
2.  **Data Privacy:** Do not paste sensitive patient data (e.g., "Patient Name: John Doe", genomic sequences that can identify individuals) into public chat interfaces.
3.  **Outdated Knowledge:** AI models have a knowledge cutoff. They might suggest deprecated functions from Seurat v3 when you are using v5.

---

## Summary
*   **Context is King:** The more specific you are about your data structure (Seurat object, metadata columns), the better the code.
*   **Verify Everything:** Treat AI code as a "draft" that you need to review and test.
*   **Use the Right Tool:** Use Antigravity for big edits in your files, and ChatGPT/Gemini for quick conceptual questions.
