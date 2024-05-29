---
layout: base
title: Tests
description: Test lists for the various components of the project.
toc: true
---

# Tests

<input type="text" id="searchBar" placeholder="Search for a test...">

<div class="dropdown">
    <button class="dropbtn">Filter</button>
    <div class="dropdown-content">
    <button class="filter-btn" data-filter="novice">Novice</button>
    <button class="filter-btn" data-filter="intermediate">Intermediate</button>
    <button class="filter-btn" data-filter="native">Native</button>
    <button id="clear-filters">Clear</button>
    </div>
</div>

<ul class="tests" id="testList"></ul>

<style>
.dropdown-content .selected {
    background-color: grey !important;
}

.dropdown {
    position: relative;
    display: inline-block;
}

.dropdown-content button {
    display: block;
}

.dropdown-content {
    display: none;
    position: relative;
    min-width: 160px;
    z-index: 1;
}

.dropdown:hover .dropdown-content {
    display: block;
}

.tests {
    font-size: 20px; /* Adjust this value to suit your needs */
}

.selected {
    background-color: grey;
}
</style>

<script>
    let tests = [];
    let filters = [];

    fetch('http://localhost:8124/api/tests/testlist')
        .then(response => response.json())
        .then(data => {
            tests = data;
            displayTests(tests);
        })
        .catch(error => console.error('Error:', error));

    document.getElementById('searchBar').addEventListener('input', (e) => {
        const searchString = e.target.value.toLowerCase();
        const filteredTests = tests.filter(test => 
            test.toLowerCase().includes(searchString) && 
            (filters.length === 0 || filters.some(filter => test.toLowerCase().includes(filter)))
        );
        displayTests(filteredTests);
    });

    document.querySelectorAll('.filter-btn').forEach(button => {
        button.addEventListener('click', (e) => {
            const filter = e.target.dataset.filter;
            const searchString = document.getElementById('searchBar').value.toLowerCase();
            if (filters.includes(filter)) {
                filters = filters.filter(f => f !== filter);
                e.target.classList.remove('selected');
            } else {
                filters.push(filter);
                e.target.classList.add('selected');
            }
            const filteredTests = tests.filter(test => 
                (filters.length === 0 || filters.some(filter => test.toLowerCase().includes(filter))) &&
                test.toLowerCase().includes(searchString)
            );
            displayTests(filteredTests);
        });
    });

    document.getElementById('clear-filters').addEventListener('click', () => {
        filters = [];
        document.querySelectorAll('.filter-btn').forEach(btn => {
            btn.classList.remove('selected');
        });
        displayTests(tests);
    });

    function displayTests(tests) {
        const testListUl = document.getElementById('testList');
        testListUl.innerHTML = '';
        tests.forEach(test => {
            const li = document.createElement('li');
            li.textContent = test;
            testListUl.appendChild(li);
        });
    }
</script>