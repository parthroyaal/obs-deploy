```txt
give me version fo this keeping stucture same with main function constisting fo flow of program and also give markdown mermaid diagram to wshow flow of main function implementation

first give me mermaid diagram mirroring flow of main function implementaion and data flow <!DOCTYPE html> <html lang="en">


```

## Output 

```txt

### Mermaid Diagram:



` ```mermaid flowchart TD     A[Start - Execute main function] --> B[Create Chart]     B --> C[Add Line Series to Chart]     C --> D[Set Data to Line Series]     D --> E[Chart Rendered]     E --> F[End] `



``This diagram represents: 1. **A (Start)**: Main function execution begins. 2. **B (Create Chart)**: A chart object is created using `LightweightCharts.createChart`. 3. **C (Add Line Series)**: A line series is added to the chart using `chart.addLineSeries`. 4. **D (Set Data)**: Data is passed to the line series using `lineSeries.setData(data)`. 5. **E (Chart Rendered)**: The chart is displayed with the provided data. 6. **F (End)**: Execution completes.  Let me know if you'd like further modifications or help!``

4o

##### You said:

also want # Implementation docs in detail to used for code from stratch without looking at original code but in highlevel not having code but high level intutive docs based on markdwon mermaid diagram nodes nodes == to no of points correspoing each points some thing like ## Create Chart Using LightweightCharts.createChart() create an obj ## Add line series to chart Using obj.addLineSeries(argsifneeded) ## Set Data to Line Series pass the data variable having list of dict with schema of each dict in list {time: iso 8601 format} to line series instance using .setData method ## Chart Rendering call main function
```

```txt
Here’s the detailed implementation documentation based on the Mermaid diagram points, presented as a high-level intuitive guide:

# Implementation Documentation

This guide outlines the step-by-step process to create a basic candlestick chart from scratch using the flow described in the Mermaid diagram.
```

```txt
now give generic systyem prompt to be given to llm model to give output for input code to get imnplemtation and diagram markdown forgiven input code the propmt should be genertic prompt irrespective of langauge etc modules used etc
```

```txt

You are a highly skilled software documentation assistant. Your task is to analyze the given input code and generate the following:

1. **Implementation Documentation**:
   - Provide a high-level, intuitive guide for implementing the functionality described in the code.
   - The documentation should include:
     - A step-by-step breakdown of the key processes or operations.
     - A description of the purpose, input, output, and behavior for each step.
     - Generic terms where applicable to ensure adaptability across similar scenarios.
   - Avoid including code snippets; instead, use descriptive and actionable text to explain how to achieve the same functionality.

2. **Mermaid Diagram**:
   - Create a Markdown Mermaid diagram that visually represents the flow of execution and data within the main function or equivalent entry point of the program.
   - Use flowchart nodes to describe the sequence of key operations or processes. Label the nodes based on their purpose or functionality, such as initializing components, processing data, or rendering output.
   - Include all key steps and transitions to illustrate the program's structure and flow.

Ensure your output is clear, concise, and well-structured. Avoid using language or details specific to the programming language or libraries beyond what is necessary for understanding.

Input Code:
[Insert code here]

```

```txt
You can provide this prompt to the LLM, and it will generate:

- A **step-by-step implementation guide** explaining the functionality of the code at a high level.
- A **Mermaid diagram** visually depicting the logical flow and data transitions in the program.

This generic system prompt ensures adaptability and usability across different domains and codebases.
```

