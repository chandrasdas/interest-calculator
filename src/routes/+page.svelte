<script>
    let initialInvestment = $state(5000);
    let interestRate = $state(14);
    let compoundFrequency = $state(4); // Quarterly
    let years = $state(5);
    let months = $state(0);
    let isDarkMode = $state(false);

    $effect(() => {
        calculateResults();
    });

    function calculateResults() {
        const r = interestRate / 100;
        const n = compoundFrequency;
        const t = years + (months / 12);
        
        // Calculate future value using compound interest formula
        // A = P(1 + r/n)^(nt)
        const futureValue = initialInvestment * Math.pow(1 + (r/n), n * t);
        const totalInterest = futureValue - initialInvestment;
        
        return {
            futureValue: futureValue.toFixed(2),
            totalInterest: totalInterest.toFixed(2),
            effectiveRate: (Math.pow(1 + r/n, n) - 1) * 100,
            totalReturn: ((futureValue / initialInvestment) - 1) * 100
        };
    }

    function toggleTheme() {
        isDarkMode = !isDarkMode;
    }
</script>

<div class="theme-toggle">
    <button onclick={toggleTheme}>
        {isDarkMode ? '☀️' : '🌙'}
    </button>
</div>

<div class="calculator" class:dark={isDarkMode}>
    <div class="inputs">
        <div class="input-group">
            <label for="initial-investment">Initial investment:</label>
            <div class="input-with-currency">
                <span>$</span>
                <input 
                    id="initial-investment"
                    type="number" 
                    bind:value={initialInvestment}
                    min="0"
                />
            </div>
        </div>

        <div class="input-group">
            <label for="interest-rate">Interest rate:</label>
            <div class="input-with-symbol">
                <input 
                    id="interest-rate"
                    type="number" 
                    bind:value={interestRate}
                    min="0"
                    max="100"
                />
                <span>%</span>
            </div>
        </div>

        <div class="input-group">
            <label for="compound-frequency">Compound frequency:</label>
            <select id="compound-frequency" bind:value={compoundFrequency}>
                <option value={1}>Annually</option>
                <option value={2}>Semi-annually</option>
                <option value={4}>Quarterly</option>
                <option value={12}>Monthly</option>
                <option value={365}>Daily</option>
            </select>
        </div>

        <div class="input-group time-inputs">
            <div>
                <label for="years">Years:</label>
                <input 
                    id="years"
                    type="number" 
                    bind:value={years}
                    min="0"
                />
            </div>
            <div>
                <label for="months">Months:</label>
                <input 
                    id="months"
                    type="number" 
                    bind:value={months}
                    min="0"
                    max="11"
                />
            </div>
        </div>
    </div>

    <div class="results">
        <h2>Results</h2>
        {#if true}
            {@const results = calculateResults()}
            <div class="result-item">
                <span class="label">Future Value:</span>
                <span>${results.futureValue}</span>
            </div>
            
            <div class="result-item">
                <span class="label">Total Interest:</span>
                <span>${results.totalInterest}</span>
            </div>
            
            <div class="result-item">
                <span class="label">Effective Annual Rate:</span>
                <span>{results.effectiveRate.toFixed(2)}%</span>
            </div>
            
            <div class="result-item">
                <span class="label">Total Return:</span>
                <span>{results.totalReturn.toFixed(2)}%</span>
            </div>
        {/if}
    </div>
</div>

<style>
    .calculator {
        max-width: 800px;
        margin: 2rem auto;
        padding: 2rem;
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 2rem;
        background: #f5f5f5;
        border-radius: 8px;
    }

    .input-group {
        margin-bottom: 1rem;
    }

    .input-with-currency,
    .input-with-symbol {
        display: flex;
        align-items: center;
    }

    .input-with-currency span,
    .input-with-symbol span {
        padding: 0.5rem;
        background: #eee;
    }

    input, select {
        padding: 0.5rem;
        width: 100%;
        border: 1px solid #ddd;
    }

    .time-inputs {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 1rem;
    }

    .results {
        background: white;
        padding: 1.5rem;
        border-radius: 4px;
    }

    .result-item {
        display: flex;
        justify-content: space-between;
        margin-bottom: 1rem;
        padding: 0.5rem;
        background: #f8f9fa;
    }

    .label {
        font-weight: 500;
    }

    .theme-toggle {
        position: fixed;
        top: 1rem;
        right: 1rem;
    }

    .theme-toggle button {
        padding: 0.5rem 1rem;
        font-size: 1.2rem;
        border: 1px solid #ddd;
        background: white;
        cursor: pointer;
        border-radius: 4px;
        transition: all 0.3s ease;
    }

    .dark {
        background: #2d3436;
        color: #f5f5f5;
    }

    .dark .results {
        background: #2d3436;
        border: 1px solid #636e72;
    }

    .dark .result-item {
        background: #353b48;
    }

    .dark input,
    .dark select,
    .dark .input-with-currency span,
    .dark .input-with-symbol span {
        background: #353b48;
        color: #f5f5f5;
        border-color: #636e72;
    }

    .dark .theme-toggle button {
        background: #353b48;
        color: #f5f5f5;
        border-color: #636e72;
    }

    @media (max-width: 768px) {
        .calculator {
            grid-template-columns: 1fr;
            margin: 1rem;
            padding: 1rem;
            gap: 1rem;
        }

        .time-inputs {
            grid-template-columns: 1fr;
            gap: 0.5rem;
        }

        .input-with-currency input,
        .input-with-symbol input {
            font-size: 16px; /* Prevents zoom on iOS */
        }

        .result-item {
            flex-direction: column;
            align-items: center;
            text-align: center;
            gap: 0.5rem;
        }

        .theme-toggle {
            top: 0.5rem;
            right: 0.5rem;
        }

        .theme-toggle button {
            padding: 0.3rem 0.6rem;
        }
    }
</style>
