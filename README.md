# mycode
An AI-powered calculator built using PHP and HTML that processes user input intelligently using AI logic to perform smart calculations.
<?php include('inc_head.php'); ?>

<title>Calculator <?php include('inc_page_name.php'); ?></title>

<?php include('inc_meta_head.php'); ?>

    <style>
        :root {
            --primary: #046a38; /* Pakistan green */
            --secondary: #01411c;
            --accent: #f9a51a; /* Pakistan orange */
            --light: #f5f5f5;
            --dark: #333;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color:white;
            color: var(--dark);
            line-height: 1.6;
        }

        

        h1 {
            text-align: center;
            color: var(--primary);
            margin-bottom: 15px;
        }

        .description {
            text-align: center;
            margin-bottom: 25px;
            color: #555;
        }

        .form-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: var(--secondary);
        }

        input, select {
            width: 100%;
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 16px;
        }

        input:focus, select:focus {
            outline: none;
            border-color: var(--accent);
        }

        .range-container {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .range-value {
            min-width: 50px;
            text-align: center;
            font-weight: bold;
        }

        button {
            background-color: var(--primary);
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
            font-weight: 600;
            width: 100%;
            transition: background-color 0.3s;
            margin-top: 10px;
        }

        button:hover {
            background-color: var(--secondary);
        }

        .result {
            margin-top: 30px;
            padding: 20px;
            background-color: #f9f9f9;
            border-radius: 5px;
            display: none;
        }

        .result h2 {
            color: var(--primary);
            margin-bottom: 15px;
        }

        .budget-item {
            display: flex;
            justify-content: space-between;
            padding: 10px 0;
            border-bottom: 1px solid #eee;
        }

        .budget-item:last-child {
            border-bottom: none;
        }

        .total {
            font-weight: bold;
            font-size: 18px;
            color: var(--secondary);
            margin-top: 10px;
        }

        .ai-recommendation {
            margin-top: 20px;
            padding: 15px;
            background-color: #e6f7ee;
            border-left: 4px solid var(--primary);
            border-radius: 0 5px 5px 0;
        }

        .tabs {
            display: flex;
            margin-bottom: 20px;
        }

        .tab {
            padding: 10px 20px;
            cursor: pointer;
            background-color: #eee;
            border: none;
            flex: 1;
            text-align: center;
            transition: all 0.3s;
        }

        .tab.active {
            background-color: var(--primary);
            color: white;
        }

        .tab-content {
            display: none;
        }

        .tab-content.active {
            display: block;
        }

        .currency-converter {
            background-color: #f0f8ff;
            padding: 15px;
            border-radius: 5px;
            margin-top: 20px;
        }

        .currency-row {
            display: flex;
            gap: 10px;
            margin-bottom: 10px;
        }

        .currency-row input {
            flex: 2;
        }

        .currency-row select {
            flex: 1;
        }

        .regions-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
            gap: 10px;
            margin-top: 10px;
        }

        .region-checkbox {
            display: flex;
            align-items: center;
            gap: 5px;
        }

        .region-checkbox input {
            width: auto;
        }

        .city-select {
            margin-top: 10px;
            display: none;
        }

        @media (max-width: 600px) {
            .container {
                margin: 15px 10px;
                padding: 15px;
            }

            .range-container {
                flex-direction: column;
                align-items: flex-start;
            }

            .regions-grid {
                grid-template-columns: 1fr 1fr;
            }

            .currency-row {
                flex-direction: column;
            }
        }
    </style>

</head>
<body>

   <?php include('inc_header.php'); ?>
   <section>
    <div class="container py-5">
        <h1>Lets Get Digital</h1>
        <p class="description">Estimate your travel expenses citywise & regionwise with currency conversion</p>

        <div class="tabs">
            <button class="tab active" onclick="openTab('calculator')">Calculator</button>
            <button class="tab" onclick="openTab('currency')">Currency Converter</button>
            <button class="tab" onclick="openTab('info')">Travel Tips</button>
        </div>

        <div id="calculator" class="tab-content active">
            <div class="form-group">
                <label for="duration">Duration of Stay (days)</label>
                <input type="number" id="duration" min="1" max="365" value="7">
            </div>

            <div class="form-group">
                <label for="regions">Select Regions You'll Visit</label>
                <div class="regions-grid">
                    <label class="region-checkbox">
                        <input type="checkbox" name="region" value="punjab" checked onchange="toggleCitySelect('punjab')"> Punjab
                    </label>
                    <label class="region-checkbox">
                        <input type="checkbox" name="region" value="sindh" onchange="toggleCitySelect('sindh')"> Sindh
                    </label>
                    <label class="region-checkbox">
                        <input type="checkbox" name="region" value="kpk" onchange="toggleCitySelect('kpk')"> KPK
                    </label>
                    <label class="region-checkbox">
                        <input type="checkbox" name="region" value="balochistan" onchange="toggleCitySelect('balochistan')"> Balochistan
                    </label>
                    <label class="region-checkbox">
                        <input type="checkbox" name="region" value="gilgit" onchange="toggleCitySelect('gilgit')"> Gilgit-Baltistan
                    </label>
                    <label class="region-checkbox">
                        <input type="checkbox" name="region" value="kashmir" onchange="toggleCitySelect('kashmir')"> Azad Kashmir
                    </label>
                </div>
            </div>

            <!-- City Selection (Dynamic) -->
            <div id="punjab-cities" class="city-select">
                <label for="punjab-city">Select Cities in Punjab</label>
                <select id="punjab-city" multiple>
                    <option value="lahore">Lahore</option>
                    <option value="islamabad">Islamabad</option>
                    <option value="rawalpindi">Rawalpindi</option>
                    <option value="multan">Multan</option>
                    <option value="faisalabad">Faisalabad</option>
                </select>
            </div>

            <div id="sindh-cities" class="city-select">
                <label for="sindh-city">Select Cities in Sindh</label>
                <select id="sindh-city" multiple>
                    <option value="karachi">Karachi</option>
                    <option value="hyderabad">Hyderabad</option>
                    <option value="sukkur">Sukkur</option>
                </select>
            </div>

            <div id="kpk-cities" class="city-select">
                <label for="kpk-city">Select Cities in KPK</label>
                <select id="kpk-city" multiple>
                    <option value="peshawar">Peshawar</option>
                    <option value="swat">Swat</option>
                    <option value="abbottabad">Abbottabad</option>
                </select>
            </div>

            <div id="balochistan-cities" class="city-select">
                <label for="balochistan-city">Select Cities in Balochistan</label>
                <select id="balochistan-city" multiple>
                    <option value="quetta">Quetta</option>
                    <option value="gwadar">Gwadar</option>
                </select>
            </div>

            <div id="gilgit-cities" class="city-select">
                <label for="gilgit-city">Select Cities in Gilgit-Baltistan</label>
                <select id="gilgit-city" multiple>
                    <option value="gilgit">Gilgit</option>
                    <option value="skardu">Skardu</option>
                    <option value="hunza">Hunza</option>
                </select>
            </div>

            <div id="kashmir-cities" class="city-select">
                <label for="kashmir-city">Select Cities in Azad Kashmir</label>
                <select id="kashmir-city" multiple>
                    <option value="muzaffarabad">Muzaffarabad</option>
                    <option value="neelum">Neelum Valley</option>
                </select>
            </div>

            <div class="form-group">
                <label for="travel-style">Travel Style</label>
                <select id="travel-style">
                    <option value="backpacker">Backpacker (Budget)</option>
                    <option value="midrange" selected>Mid-range</option>
                    <option value="luxury">Luxury</option>
                </select>
            </div>

            <div class="form-group">
                <label for="accommodation">Accommodation Type</label>
                <select id="accommodation">
                    <option value="hostel">Hostel/Dormitory</option>
                    <option value="guesthouse" selected>Guesthouse/3-star Hotel</option>
                    <option value="hotel">4-5 Star Hotel</option>
                    <option value="resort">Luxury Resort</option>
                </select>
            </div>

            <div class="form-group">
                <label for="transport">Transportation Preference</label>
                <select id="transport">
                    <option value="public">Public Transport</option>
                    <option value="mix" selected>Mix of Public and Private</option>
                    <option value="private">Private Transport/Rental</option>
                    <option value="driver">Private Car with Driver</option>
                </select>
            </div>

            <div class="form-group">
                <label for="food">Food Preference</label>
                <select id="food">
                    <option value="street">Street Food/Local Eateries</option>
                    <option value="mix" selected>Mix of Street and Restaurants</option>
                    <option value="restaurants">Restaurants Only</option>
                    <option value="fine-dining">Fine Dining</option>
                </select>
            </div>

            <div class="form-group">
                <label for="activities">Activities Level</label>
                <div class="range-container">
                    <input type="range" id="activities" min="1" max="5" value="3">
                    <span class="range-value" id="activities-value">Moderate</span>
                </div>
            </div>

            <button onclick="calculateBudget()">Calculate Budget</button>

            <div id="result" class="result">
                <h2>Estimated Budget</h2>
                <div id="region-results">
                    <!-- Region-specific results will appear here -->
                </div>
                <div class="budget-item total">
                    <span>Total Estimated Budget:</span>
                    <span id="total-cost">$0</span>
                </div>

                <div class="ai-recommendation" id="recommendation">
                    <!-- AI recommendations will appear here -->
                </div>
            </div>
        </div>

        <div id="currency" class="tab-content">
            <h2>Currency Converter</h2>
            <div class="currency-converter">
                <div class="currency-row">
                    <input type="number" id="currency-amount" placeholder="Amount" value="100">
                    <select id="currency-from">
                        <option value="USD">US Dollar (USD)</option>
                        <option value="EUR">Euro (EUR)</option>
                        <option value="GBP">British Pound (GBP)</option>
                        <option value="PKR" selected>Pakistani Rupee (PKR)</option>
                        <option value="AED">UAE Dirham (AED)</option>
                        <option value="SAR">Saudi Riyal (SAR)</option>
                        <option value="CNY">Chinese Yuan (CNY)</option>
                        <option value="INR">Indian Rupee (INR)</option>
                    </select>
                </div>
                <div class="currency-row">
                    <input type="number" id="currency-result" placeholder="Result" readonly>
                    <select id="currency-to">
                        <option value="USD" selected>US Dollar (USD)</option>
                        <option value="EUR">Euro (EUR)</option>
                        <option value="GBP">British Pound (GBP)</option>
                        <option value="PKR">Pakistani Rupee (PKR)</option>
                        <option value="AED">UAE Dirham (AED)</option>
                        <option value="SAR">Saudi Riyal (SAR)</option>
                        <option value="CNY">Chinese Yuan (CNY)</option>
                        <option value="INR">Indian Rupee (INR)</option>
                    </select>
                </div>
                <button onclick="convertCurrency()">Convert</button>
                <div id="conversion-rate" style="margin-top: 10px; font-size: 0.9em; color: #555;"></div>
            </div>

            <div class="ai-recommendation" style="margin-top: 20px;">
                <h3>Money Tips for Pakistan</h3>
                <ul>
                    <li>Exchange money at authorized dealers for best rates (avoid airports)</li>
                    <li>ATMs are widely available in cities but carry cash in remote areas</li>
                    <li>Credit cards accepted in major hotels and restaurants</li>
                    <li>Small denominations are helpful for tips and small purchases</li>
                    <li>Current approximate rates: 1 USD ≈ 280 PKR, 1 EUR ≈ 300 PKR, 1 GBP ≈ 350 PKR</li>
                </ul>
            </div>
        </div>

        <div id="info" class="tab-content">
            <h2>Pakistan Travel Tips</h2>
            <div class="ai-recommendation">
                <h3>Regional Price Variations</h3>
                <table style="width:100%; border-collapse: collapse; margin: 15px 0;">
                    <tr style="background-color: var(--primary); color: white;">
                        <th style="padding: 8px; text-align: left;">Region</th>
                        <th style="padding: 8px; text-align: left;">Backpacker</th>
                        <th style="padding: 8px; text-align: left;">Mid-range</th>
                        <th style="padding: 8px; text-align: left;">Luxury</th>
                    </tr>
                    <tr style="border-bottom: 1px solid #ddd;">
                        <td style="padding: 8px;">Punjab (Lahore, Islamabad)</td>
                        <td style="padding: 8px;">$15-$25</td>
                        <td style="padding: 8px;">$40-$80</td>
                        <td style="padding: 8px;">$120-$300</td>
                    </tr>
                    <tr style="border-bottom: 1px solid #ddd;">
                        <td style="padding: 8px;">Sindh (Karachi)</td>
                        <td style="padding: 8px;">$15-$30</td>
                        <td style="padding: 8px;">$50-$100</td>
                        <td style="padding: 8px;">$150-$350</td>
                    </tr>
                    <tr style="border-bottom: 1px solid #ddd;">
                        <td style="padding: 8px;">KPK (Peshawar, Swat)</td>
                        <td style="padding: 8px;">$15-$25</td>
                        <td style="padding: 8px;">$40-$90</td>
                        <td style="padding: 8px;">$120-$250</td>
                    </tr>
                    <tr style="border-bottom: 1px solid #ddd;">
                        <td style="padding: 8px;">Balochistan (Quetta)</td>
                        <td style="padding: 8px;">$10-$20</td>
                        <td style="padding: 8px;">$30-$60</td>
                        <td style="padding: 8px;">$100-$200</td>
                    </tr>
                    <tr style="border-bottom: 1px solid #ddd;">
                        <td style="padding: 8px;">Gilgit-Baltistan</td>
                        <td style="padding: 8px;">$25-$40</td>
                        <td style="padding: 8px;">$70-$150</td>
                        <td style="padding: 8px;">$200-$400+</td>
                    </tr>
                    <tr>
                        <td style="padding: 8px;">Azad Kashmir</td>
                        <td style="padding: 8px;">$20-$35</td>
                        <td style="padding: 8px;">$60-$120</td>
                        <td style="padding: 8px;">$180-$350</td>
                    </tr>
                </table>

                <h3>Best Time to Visit</h3>
                <ul>
                    <li><strong>North (Gilgit, Hunza, Kashmir):</strong> May-September (best weather)</li>
                    <li><strong>Punjab & Sindh:</strong> October-March (avoid summer heat)</li>
                    <li><strong>KPK (Swat, Peshawar):</strong> April-June & September-November</li>
                </ul>

                <h3>Must-Visit Places</h3>
                <ul>
                    <li><strong>Lahore:</strong> Badshahi Mosque, Food Street</li>
                    <li><strong>Islamabad:</strong> Faisal Mosque, Margalla Hills</li>
                    <li><strong>Karachi:</strong> Clifton Beach, Mohatta Palace</li>
                    <li><strong>Hunza Valley:</strong> Baltit Fort, Attabad Lake</li>
                    <li><strong>Swat Valley:</strong> Malam Jabba, White Palace</li>
                </ul>
            </div>
        </div>
    </div>
   </section>

   <script>
    // Exchange rates (simplified - in real app fetch from API)
    const exchangeRates = {
        USD: { rate: 1, symbol: '$' },
        EUR: { rate: 0.93, symbol: '€' },
        GBP: { rate: 0.79, symbol: '£' },
        PKR: { rate: 280, symbol: 'Rs ' },
        AED: { rate: 3.67, symbol: 'AED ' },
        SAR: { rate: 3.75, symbol: 'SAR ' },
        CNY: { rate: 7.23, symbol: '¥' },
        INR: { rate: 83.29, symbol: '₹' }
    };

    // Regional & city price multipliers
    const regionMultipliers = {
        punjab: { 
            base: { accommodation: 1.0, food: 1.0, transport: 1.0, activities: 1.0 },
            cities: {
                lahore: { accommodation: 1.1, food: 1.2, transport: 1.1, activities: 1.1 },
                islamabad: { accommodation: 1.2, food: 1.1, transport: 1.0, activities: 1.0 },
                rawalpindi: { accommodation: 0.9, food: 0.9, transport: 0.9, activities: 0.8 },
                multan: { accommodation: 0.8, food: 0.8, transport: 0.8, activities: 0.7 },
                faisalabad: { accommodation: 0.8, food: 0.8, transport: 0.8, activities: 0.7 }
            }
        },
        sindh: { 
            base: { accommodation: 1.1, food: 1.0, transport: 1.2, activities: 1.0 },
            cities: {
                karachi: { accommodation: 1.3, food: 1.2, transport: 1.3, activities: 1.2 },
                hyderabad: { accommodation: 0.9, food: 0.9, transport: 0.9, activities: 0.8 },
                sukkur: { accommodation: 0.8, food: 0.8, transport: 0.8, activities: 0.7 }
            }
        },
        kpk: { 
            base: { accommodation: 1.0, food: 0.9, transport: 1.1, activities: 1.1 },
            cities: {
                peshawar: { accommodation: 1.1, food: 1.0, transport: 1.1, activities: 1.0 },
                swat: { accommodation: 1.2, food: 1.1, transport: 1.2, activities: 1.3 },
                abbottabad: { accommodation: 0.9, food: 0.9, transport: 0.9, activities: 0.9 }
            }
        },
        balochistan: { 
            base: { accommodation: 0.8, food: 0.7, transport: 1.3, activities: 0.7 },
            cities: {
                quetta: { accommodation: 0.9, food: 0.8, transport: 1.1, activities: 0.8 },
                gwadar: { accommodation: 1.0, food: 0.9, transport: 1.2, activities: 0.9 }
            }
        },
        gilgit: { 
            base: { accommodation: 1.5, food: 1.3, transport: 1.4, activities: 1.5 },
            cities: {
                gilgit: { accommodation: 1.4, food: 1.2, transport: 1.3, activities: 1.2 },
                skardu: { accommodation: 1.6, food: 1.4, transport: 1.5, activities: 1.6 },
                hunza: { accommodation: 1.7, food: 1.5, transport: 1.6, activities: 1.8 }
            }
        },
        kashmir: { 
            base: { accommodation: 1.3, food: 1.1, transport: 1.3, activities: 1.3 },
            cities: {
                muzaffarabad: { accommodation: 1.2, food: 1.0, transport: 1.2, activities: 1.1 },
                neelum: { accommodation: 1.4, food: 1.2, transport: 1.4, activities: 1.5 }
            }
        }
    };

    // Region names for display
    const regionNames = {
        punjab: "Punjab",
        sindh: "Sindh",
        kpk: "Khyber Pakhtunkhwa",
        balochistan: "Balochistan",
        gilgit: "Gilgit-Baltistan",
        kashmir: "Azad Kashmir"
    };

    // City names for display
    const cityNames = {
        lahore: "Lahore",
        islamabad: "Islamabad",
        rawalpindi: "Rawalpindi",
        multan: "Multan",
        faisalabad: "Faisalabad",
        karachi: "Karachi",
        hyderabad: "Hyderabad",
        sukkur: "Sukkur",
        peshawar: "Peshawar",
        swat: "Swat",
        abbottabad: "Abbottabad",
        quetta: "Quetta",
        gwadar: "Gwadar",
        gilgit: "Gilgit",
        skardu: "Skardu",
        hunza: "Hunza",
        muzaffarabad: "Muzaffarabad",
        neelum: "Neelum Valley"
    };

    // Initialize the activities slider
    const activitiesSlider = document.getElementById('activities');
    const activitiesValue = document.getElementById('activities-value');
    
    activitiesSlider.addEventListener('input', function() {
        const value = parseInt(this.value);
        let text = '';
        
        switch(value) {
            case 1: text = 'Minimal'; break;
            case 2: text = 'Light'; break;
            case 3: text = 'Moderate'; break;
            case 4: text = 'Active'; break;
            case 5: text = 'Intensive'; break;
            default: text = 'Moderate';
        }
        
        activitiesValue.textContent = text;
    });

    // Tab functionality
    function openTab(tabName) {
        const tabs = document.getElementsByClassName('tab');
        const tabContents = document.getElementsByClassName('tab-content');
        
        for (let i = 0; i < tabs.length; i++) {
            tabs[i].classList.remove('active');
            tabContents[i].classList.remove('active');
        }
        
        document.getElementById(tabName).classList.add('active');
        event.currentTarget.classList.add('active');
    }

    // Toggle city selection based on region
    function toggleCitySelect(region) {
        const citySelectDiv = document.getElementById(`${region}-cities`);
        const checkbox = document.querySelector(`input[name="region"][value="${region}"]`);
        
        if (checkbox.checked) {
            citySelectDiv.style.display = 'block';
        } else {
            citySelectDiv.style.display = 'none';
            // Clear selected cities if region is unchecked
            const citySelect = document.getElementById(`${region}-city`);
            if (citySelect) {
                citySelect.selectedIndex = -1;
            }
        }
    }

    // Calculate budget with regional & city variations
    function calculateBudget() {
        // Get user inputs
        const duration = parseInt(document.getElementById('duration').value);
        const travelStyle = document.getElementById('travel-style').value;
        const accommodation = document.getElementById('accommodation').value;
        const transport = document.getElementById('transport').value;
        const food = document.getElementById('food').value;
        const activitiesLevel = parseInt(document.getElementById('activities').value);

        // Get selected regions
        const regionCheckboxes = document.querySelectorAll('input[name="region"]:checked');
        const selectedRegions = Array.from(regionCheckboxes).map(cb => cb.value);

        if (selectedRegions.length === 0) {
            alert("Please select at least one region");
            return;
        }

        // Get selected cities for each region
        const selectedCities = {};
        selectedRegions.forEach(region => {
            const citySelect = document.getElementById(`${region}-city`);
            if (citySelect) {
                const cities = Array.from(citySelect.selectedOptions).map(opt => opt.value);
                if (cities.length > 0) {
                    selectedCities[region] = cities;
                }
            }
        });

        // Calculate days per region (simple equal distribution)
        const daysPerRegion = duration / selectedRegions.length;
        
        let totalBudget = 0;
        let regionResultsHTML = '';

        // Calculate budget for each region and city
        selectedRegions.forEach(region => {
            const regionData = regionMultipliers[region];
            const citiesInRegion = selectedCities[region] || [];

            // If no cities selected, use region base
            if (citiesInRegion.length === 0) {
                const regionalCost = calculateRegionalCost(
                    regionData.base,
                    daysPerRegion,
                    travelStyle,
                    accommodation,
                    transport,
                    food,
                    activitiesLevel
                );
                
                totalBudget += regionalCost.total;
                regionResultsHTML += generateRegionHTML(region, null, regionalCost, daysPerRegion);
            } else {
                // Calculate for each city in the region
                const daysPerCity = daysPerRegion / citiesInRegion.length;
                let regionSubtotal = 0;

                citiesInRegion.forEach(city => {
                    const cityMultiplier = regionData.cities[city];
                    // Combine region base with city-specific multipliers
                    const combinedMultiplier = {
                        accommodation: regionData.base.accommodation * cityMultiplier.accommodation,
                        food: regionData.base.food * cityMultiplier.food,
                        transport: regionData.base.transport * cityMultiplier.transport,
                        activities: regionData.base.activities * cityMultiplier.activities
                    };

                    const cityCost = calculateRegionalCost(
                        combinedMultiplier,
                        daysPerCity,
                        travelStyle,
                        accommodation,
                        transport,
                        food,
                        activitiesLevel
                    );

                    regionSubtotal += cityCost.total;
                    regionResultsHTML += generateRegionHTML(region, city, cityCost, daysPerCity);
                });

                totalBudget += regionSubtotal;
            }
        });

        // Display results
        document.getElementById('region-results').innerHTML = regionResultsHTML;
        document.getElementById('total-cost').textContent = `$${totalBudget.toFixed(2)}`;

        // Generate AI recommendations
        generateRecommendations(
            duration, 
            travelStyle, 
            accommodation, 
            transport, 
            food, 
            activitiesLevel, 
            totalBudget,
            selectedRegions,
            selectedCities
        );

        // Show results
        document.getElementById('result').style.display = 'block';
        document.getElementById('result').scrollIntoView({ behavior: 'smooth' });
    }

    // Calculate costs for a region or city
    function calculateRegionalCost(multiplier, days, travelStyle, accommodation, transport, food, activitiesLevel) {
        // Accommodation costs
        let accommodationCost;
        switch(accommodation) {
            case 'hostel':
                accommodationCost = (5 + (travelStyle === 'luxury' ? 5 : 0)) * multiplier.accommodation;
                break;
            case 'guesthouse':
                accommodationCost = (20 + (travelStyle === 'luxury' ? 10 : (travelStyle === 'backpacker' ? -5 : 0))) * multiplier.accommodation;
                break;
            case 'hotel':
                accommodationCost = (60 + (travelStyle === 'luxury' ? 40 : (travelStyle === 'backpacker' ? -20 : 0))) * multiplier.accommodation;
                break;
            case 'resort':
                accommodationCost = (120 + (travelStyle === 'luxury' ? 80 : (travelStyle === 'backpacker' ? -60 : 0))) * multiplier.accommodation;
                break;
            default:
                accommodationCost = 20 * multiplier.accommodation;
        }

        // Food costs
        let foodCost;
        switch(food) {
            case 'street':
                foodCost = (5 + (travelStyle === 'luxury' ? 3 : 0)) * multiplier.food;
                break;
            case 'mix':
                foodCost = (10 + (travelStyle === 'luxury' ? 5 : (travelStyle === 'backpacker' ? -3 : 0))) * multiplier.food;
                break;
            case 'restaurants':
                foodCost = (20 + (travelStyle === 'luxury' ? 10 : (travelStyle === 'backpacker' ? -5 : 0))) * multiplier.food;
                break;
            case 'fine-dining':
                foodCost = (40 + (travelStyle === 'luxury' ? 20 : (travelStyle === 'backpacker' ? -15 : 0))) * multiplier.food;
                break;
            default:
                foodCost = 10 * multiplier.food;
        }

        // Transport costs
        let transportCost;
        switch(transport) {
            case 'public':
                transportCost = (3 + (travelStyle === 'luxury' ? 2 : 0)) * multiplier.transport;
                break;
            case 'mix':
                transportCost = (10 + (travelStyle === 'luxury' ? 5 : (travelStyle === 'backpacker' ? -4 : 0))) * multiplier.transport;
                break;
            case 'private':
                transportCost = (25 + (travelStyle === 'luxury' ? 15 : (travelStyle === 'backpacker' ? -10 : 0))) * multiplier.transport;
                break;
            case 'driver':
                transportCost = (40 + (travelStyle === 'luxury' ? 20 : (travelStyle === 'backpacker' ? -15 : 0))) * multiplier.transport;
                break;
            default:
                transportCost = 10 * multiplier.transport;
        }

        // Activities costs (multiplier)
        const activitiesMultiplier = activitiesLevel * (travelStyle === 'luxury' ? 1.5 : (travelStyle === 'backpacker' ? 0.6 : 1));
        let activitiesCost = (5 * activitiesMultiplier) * multiplier.activities;

        // Miscellaneous costs (10-20% of other costs)
        const baseDailyCost = accommodationCost + foodCost + transportCost + activitiesCost;
        let miscCost = baseDailyCost * (travelStyle === 'luxury' ? 0.2 : (travelStyle === 'backpacker' ? 0.1 : 0.15));

        // Calculate totals
        const dailyCost = accommodationCost + foodCost + transportCost + activitiesCost + miscCost;
        const totalCost = dailyCost * days;

        return {
            accommodation: accommodationCost * days,
            food: foodCost * days,
            transport: transportCost * days,
            activities: activitiesCost * days,
            misc: miscCost * days,
            total: totalCost,
            daily: dailyCost
        };
    }

    // Generate HTML for region/city results
    function generateRegionHTML(region, city, costs, days) {
        const locationName = city ? `${cityNames[city]} (${regionNames[region]})` : regionNames[region];
        
        return `
            <div style="margin-bottom: 15px; border-bottom: 1px dashed #ccc; padding-bottom: 10px;">
                <h3 style="color: var(--primary); margin-bottom: 5px;">${locationName}</h3>
                <div class="budget-item">
                    <span>Accommodation (${days.toFixed(1)} days):</span>
                    <span>$${costs.accommodation.toFixed(2)}</span>
                </div>
                <div class="budget-item">
                    <span>Food:</span>
                    <span>$${costs.food.toFixed(2)}</span>
                </div>
                <div class="budget-item">
                    <span>Transportation:</span>
                    <span>$${costs.transport.toFixed(2)}</span>
                </div>
                <div class="budget-item">
                    <span>Activities:</span>
                    <span>$${costs.activities.toFixed(2)}</span>
                </div>
                <div class="budget-item">
                    <span>Miscellaneous:</span>
                    <span>$${costs.misc.toFixed(2)}</span>
                </div>
                <div class="budget-item" style="font-weight: bold;">
                    <span>Subtotal for ${locationName}:</span>
                    <span>$${costs.total.toFixed(2)}</span>
                </div>
            </div>
        `;
    }

    // AI recommendation generator with regional & city focus
    function generateRecommendations(duration, travelStyle, accommodation, transport, food, activitiesLevel, totalBudget, regions, cities) {
        let recommendations = '<h3>AI Recommendations</h3>';
        const dailyBudget = totalBudget / duration;
        const visitingNorth = regions.includes('gilgit') || regions.includes('kashmir');
        const visitingSouth = regions.includes('sindh') || regions.includes('balochistan');
        const visitingCities = regions.includes('punjab') || regions.includes('sindh');

        // General recommendation based on budget
        if (dailyBudget < 30) {
            recommendations += '<p>You\'re planning a very budget-conscious trip. Consider these tips:</p><ul>';
            recommendations += '<li>Hostels and guesthouses in northern areas often include breakfast</li>';
            recommendations += '<li>Travel by bus between cities for the best rates (Daewoo and Faisal Movers are reliable options)</li>';
            recommendations += '<li>Local eateries offer delicious food at low prices - try halwa puri for breakfast</li>';
            recommendations += '</ul>';
        } else if (dailyBudget < 100) {
            recommendations += '<p>Your mid-range budget allows for comfortable travel. Suggestions:</p><ul>';
            recommendations += '<li>Mix hotels with some guesthouse stays to balance comfort and budget</li>';
            if (visitingNorth && visitingSouth) {
                recommendations += '<li>Consider domestic flights for long distances (Islamabad to Karachi saves 24+ hours)</li>';
            }
            recommendations += '<li>Try both street food and mid-range restaurants for variety</li>';
            recommendations += '</ul>';
        } else {
            recommendations += '<p>With your luxury budget, you can enjoy premium experiences:</p><ul>';
            recommendations += '<li>Book heritage hotels in Lahore (e.g., Pearl Continental) and Karachi for unique stays</li>';
            if (visitingNorth) {
                recommendations += '<li>Consider luxury camping in the northern mountains (Hunza and Skardu have excellent options)</li>';
            }
            recommendations += '</ul>';
        }

        // City-specific recommendations
        if (cities.punjab?.includes('lahore')) {
            recommendations += '<h4>Lahore Tips</h4><ul>';
            recommendations += '<li>Don\'t miss Food Street in Old Lahore for authentic cuisine</li>';
            recommendations += '<li>Visit Badshahi Mosque early to avoid crowds</li>';
            recommendations += '</ul>';
        }

        if (cities.gilgit?.includes('hunza')) {
            recommendations += '<h4>Hunza Valley Tips</h4><ul>';
            recommendations += '<li>Book accommodations in advance during peak season (June-September)</li>';
            recommendations += '<li>Visit Altit and Baltit Forts for amazing views</li>';
            recommendations += '</ul>';
        }

        if (cities.sindh?.includes('karachi')) {
            recommendations += '<h4>Karachi Tips</h4><ul>';
            recommendations += '<li>Try Burns Road for the best street food</li>';
            recommendations += '<li>Visit Clifton Beach in the evening for cooler weather</li>';
            recommendations += '</ul>';
        }

        // Transport between regions
        if (regions.length > 1) {
            recommendations += '<h4>Inter-Region Travel</h4><ul>';
            if (visitingNorth && visitingSouth) {
                recommendations += '<li>Flights between Islamabad and Karachi are frequent and time-saving</li>';
            }
            if (regions.includes('punjab') && regions.includes('kpk')) {
                recommendations += '<li>Peshawar-Lahore highway is in good condition (5-6 hour drive)</li>';
            }
            recommendations += '<li>Train travel is affordable but slow - consider night trains for long distances</li>';
            recommendations += '</ul>';
        }

        // Food recommendations by region
        recommendations += '<h4>Must-Try Foods</h4><ul>';
        if (regions.includes('punjab')) {
            recommendations += '<li>Lahore: Try food street in Old Lahore (especially Phajja Siri Paye)</li>';
        }
        if (regions.includes('sindh')) {
            recommendations += '<li>Karachi: Don\'t miss Burns Road for authentic Sindhi biryani</li>';
        }
        if (regions.includes('kpk')) {
            recommendations += '<li>Peshawar: Sample chapli kabab and Peshawari naan</li>';
        }
        if (visitingNorth) {
            recommendations += '<li>Northern Areas: Try local apricot products and walnuts</li>';
        }
        recommendations += '</ul>';

        document.getElementById('recommendation').innerHTML = recommendations;
    }

    // Currency conversion
    function convertCurrency() {
        const amount = parseFloat(document.getElementById('currency-amount').value);
        const fromCurrency = document.getElementById('currency-from').value;
        const toCurrency = document.getElementById('currency-to').value;
        
        if (isNaN(amount)) {
            alert("Please enter a valid amount");
            return;
        }
        
        // Convert to USD first, then to target currency
        const usdAmount = amount / exchangeRates[fromCurrency].rate;
        const convertedAmount = usdAmount * exchangeRates[toCurrency].rate;
        
        document.getElementById('currency-result').value = convertedAmount.toFixed(2);
        
        // Show conversion rate
        const rate = (exchangeRates[toCurrency].rate / exchangeRates[fromCurrency].rate).toFixed(4);
        document.getElementById('conversion-rate').innerHTML = 
            `1 ${fromCurrency} = ${rate} ${toCurrency}`;
    }

    // Initialize the activities slider
    activitiesSlider.dispatchEvent(new Event('input'));

    // Set up currency converter to convert when either input changes
    document.getElementById('currency-amount').addEventListener('input', convertCurrency);
    document.getElementById('currency-from').addEventListener('change', convertCurrency);
    document.getElementById('currency-to').addEventListener('change', convertCurrency);

    // Initial currency conversion
    convertCurrency();
</script>

<?php include('inc_footer.php'); ?>
<?php include('inc_foot.php'); ?>

