---
layout: base 
title: Placement Test
search_exclude: true
---

<style>
.placer-container {
    display: flex;
    justify-content: space-between;
}

.placer-form {
    width: 45%;
}
</style>

<div class="placer-container">
<!-- Python Placement Test Form -->
    <form action="javascript:placer_usr()">
        <p>
            <button>Start Test</button>
        </p>
    </form>
    <p id="python-message" style="color: white;"></p>
    <script>
        // spanish placement test
        // grammar, vocabulary, and reading.
        // max of 10 questions each
        // max of 30 questions total
        // questions varry from level  1-4 where 1 is novice and 4 is expert
        // questions are multiple choice
        // goes for each section till the user gets 3 wrong then moves to the next section
        fetch('questions.csv')
            .then(response => response.text())
            .then(data => {
                const lines = data.split('\n');
                const questions = lines.map(line => line.split(','));
                // Now you can use the questions array in your application
            });