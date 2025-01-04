<!DOCTYPE html>
<html>
<head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Arial, sans-serif;
        }

        :root {
            --primary-color: #4CAF50;
            --secondary-color: #45a049;
            --danger-color: #f44336;
            --warning-color: #FFC107;
            --light-bg: #f8f9fa;
            --border-color: #e1e4e8;
            --shadow: 0 2px 10px rgba(0,0,0,0.05);
            --transition: all 0.3s ease;
        }

        body {
            background-color: #f5f5f5;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
            padding: 20px;
        }

        .header {
            background: white;
            padding: 20px;
            border-radius: 12px;
            margin-bottom: 20px;
            box-shadow: var(--shadow);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .header h1 {
            font-size: 1.8rem;
            color: #333;
        }

        .view-toggle {
            display: flex;
            gap: 10px;
        }

        .view-button {
            padding: 8px 15px;
            border: 2px solid var(--border-color);
            border-radius: 6px;
            background: white;
            cursor: pointer;
            transition: var(--transition);
        }

        .view-button.active {
            background: var(--primary-color);
            color: white;
            border-color: var(--primary-color);
        }

        .search-section {
            background: white;
            padding: 20px;
            border-radius: 12px;
            margin-bottom: 20px;
            box-shadow: var(--shadow);
        }

        .search-container {
            display: flex;
            gap: 15px;
            margin-bottom: 20px;
            position: relative;
        }

        .search-bar {
            flex: 1;
            padding: 12px 20px;
            padding-left: 40px;
            border: 2px solid var(--border-color);
            border-radius: 8px;
            font-size: 16px;
            transition: var(--transition);
        }

        .search-icon {
            position: absolute;
            left: 12px;
            top: 50%;
            transform: translateY(-50%);
            color: #666;
        }

        .filters {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
        }

        .filter-group {
            position: relative;
        }

        select {
            width: 100%;
            padding: 12px 15px;
            border: 2px solid var(--border-color);
            border-radius: 8px;
            background: white;
            cursor: pointer;
            appearance: none;
        }

        .filter-group::after {
            content: '▼';
            position: absolute;
            right: 15px;
            top: 50%;
            transform: translateY(-50%);
            pointer-events: none;
            font-size: 12px;
            color: #666;
        }

        .plot-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .plot {
            background: white;
            border-radius: 12px;
            box-shadow: var(--shadow);
            cursor: pointer;
            transition: var(--transition);
            padding: 20px;
            position: relative;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .plot:hover {
            transform: translateY(-5px);
            box-shadow: 0 5px 20px rgba(0,0,0,0.1);
        }

        .plot-status {
            position: absolute;
            top: 15px;
            right: 15px;
            padding: 4px 8px;
            border-radius: 4px;
            font-size: 0.8em;
            font-weight: 600;
        }

        .status-available {
            background: rgba(76, 175, 80, 0.1);
            color: var(--primary-color);
        }

        .status-sold {
            background: rgba(244, 67, 54, 0.1);
            color: var(--danger-color);
        }

        .status-reserved {
            background: rgba(255, 193, 7, 0.1);
            color: var(--warning-color);
        }

        .plot-image {
            width: 100%;
            height: 120px;
            background: var(--light-bg);
            border-radius: 8px;
            margin-bottom: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #666;
        }

        .plot-info {
            display: flex;
            flex-direction: column;
            gap: 5px;
        }

        .plot-number {
            font-weight: 600;
            font-size: 1.1em;
            color: #333;
        }

        .plot-size, .plot-price {
            font-size: 0.9em;
            color: #666;
        }

        .plot-features {
            display: flex;
            gap: 10px;
            margin-top: 10px;
        }

        .feature-tag {
            background: var(--light-bg);
            padding: 4px 8px;
            border-radius: 4px;
            font-size: 0.8em;
            color: #666;
        }

        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            z-index: 1000;
            backdrop-filter: blur(5px);
        }

        .modal-content {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: white;
            padding: 30px;
            border-radius: 16px;
            width: 90%;
            max-width: 800px;
            max-height: 90vh;
            overflow-y: auto;
        }

        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 25px;
            padding-bottom: 15px;
            border-bottom: 2px solid var(--border-color);
        }

        .close-modal {
            cursor: pointer;
            font-size: 24px;
            color: #666;
            transition: var(--transition);
        }

        .close-modal:hover {
            color: #333;
        }

        .plot-details {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 20px;
            margin-bottom: 25px;
        }

        .detail-item {
            background: var(--light-bg);
            padding: 20px;
            border-radius: 12px;
            transition: var(--transition);
        }

        .detail-item:hover {
            background: #f0f0f0;
        }

        .detail-label {
            color: #666;
            margin-bottom: 8px;
            font-size: 0.9em;
        }

        .detail-value {
            font-weight: 600;
            font-size: 1.1em;
            color: #333;
        }

        .action-buttons {
            display: flex;
            gap: 15px;
            margin-top: 25px;
        }

        .btn {
            flex: 1;
            padding: 15px 25px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-weight: 600;
            transition: var(--transition);
            text-align: center;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }

        .btn-primary {
            background: var(--primary-color);
            color: white;
        }

        .btn-primary:hover {
            background: var(--secondary-color);
        }

        .btn-secondary {
            background: var(--light-bg);
            color: #333;
            border: 2px solid var(--border-color);
        }

        .btn-secondary:hover {
            background: #e9ecef;
        }

        .plot-gallery {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 15px;
            margin: 20px 0;
        }

        .gallery-img {
            aspect-ratio: 16/9;
            background: var(--light-bg);
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #666;
        }

        .plot-description {
            margin: 20px 0;
            line-height: 1.6;
            color: #666;
        }

        .amenities {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            gap: 15px;
            margin: 20px 0;
        }

        .amenity-item {
            display: flex;
            align-items: center;
            gap: 8px;
            padding: 10px;
            background: var(--light-bg);
            border-radius: 6px;
            font-size: 0.9em;
            color: #666;
        }

        .progress-bar {
            height: 6px;
            background: var(--light-bg);
            border-radius: 3px;
            overflow: hidden;
            margin-top: 5px;
        }

        .progress-fill {
            height: 100%;
            background: var(--primary-color);
            transition: var(--transition);
        }

        @media (max-width: 768px) {
            .container {
                padding: 10px;
            }

            .header {
                flex-direction: column;
                gap: 15px;
                text-align: center;
            }

            .search-container {
                flex-direction: column;
            }

            .plot-grid {
                grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
            }

            .plot-details {
                grid-template-columns: 1fr;
            }

            .action-buttons {
                flex-direction: column;
            }

            .plot-gallery {
                grid-template-columns: 1fr;
            }

            .modal-content {
                padding: 20px;
            }
        }

        @media (max-width: 480px) {
            .plot-grid {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>Premium Plot Finder</h1>
            <div class="view-toggle">
                <button class="view-button active" data-view="grid">Grid View</button>
                <button class="view-button" data-view="map">Map View</button>
            </div>
        </div>

        <div class="search-section">
            <div class="search-container">
                <span class="search-icon">🔍</span>
                <input type="text" class="search-bar" placeholder="Search by plot number, size, or location...">
            </div>
            <div class="filters">
                <div class="filter-group">
                    <select id="phase-filter">
                        <option value="">Select Phase</option>
                        <option value="phase-a">Phase A</option>
                        <option value="phase-b">Phase B</option>
                        <option value="phase-c">Phase C</option>
                    </select>
                </div>
                <div class="filter-group">
                    <select id="status-filter">
                        <option value="">Plot Status</option>
                        <option value="available">Available</option>
                        <option value="sold">Sold</option>
                        <option value="reserved">Reserved</option>
                    </select>
                </div>
                <div class="filter-group">
                    <select id="size-filter">
                        <option value="">Plot Size</option>
                        <option value="small">Small (< 1000 sq ft)</option>
                        <option value="medium">Medium (1000-2000 sq ft)</option>
                        <option value="large">Large (> 2000 sq ft)</option>
                    </select>
                </div>
                <div class="filter-group">
                    <select id="price-filter">
                        <option value="">Price Range</option>
                        <option value="0-500000">Under $500,000</option>
                        <option value="500000-1000000">$500,000 - $1,000,000</option>
                        <option value="1000000+">Above $1,000,000</option>
                    </select>
                </div>
            </div>
        </div>

        <div id="plot-grid" class="plot-grid">
            <!-- Plots will be generated here -->
        </div>
    </div>

    <div id="plot-modal" class="modal">
        <div class="modal-content">
            <div class="modal-header">
                <h2>Plot Details</h2>
                <span class="close-modal">&times;</span>
            </div>
            <div class="plot-gallery">
                <!-- Gallery images will be added here -->
            </div>
            <div class="plot-description">
                <!-- Description will be added here -->
            </div>
            <div class="plot-details">
                <!-- Details will be populated via JavaScript -->
            </div>
            <div class="amenities">
                <!-- Amenities will be added here -->
            </div>
            <div class="action-buttons">
                <button class="btn btn-primary" onclick="handleBooking()">
                    <span>📅</span> Book Now
                </button>
                <button class="btn btn-secondary" onclick="scheduleVisit()">
                    <span>👁️</span> Schedule Visit
                </button>
                <button class="btn btn-secondary" onclick="downloadBrochure()">
                    <span>📄</span> Download Brochure
                </button>
            </div>
        </div>
    </div>

    <script>
        // Generate plot data
        const generatePlots = () => {
            const grid = document.getElementById('plot-grid');
            const statuses = ['available', 'sold', 'reserved'];
            const statusLabels = {
                'available': 'Available',
                'sold': 'Sold',
                'reserved': 'Reserved'
            };
            
            const directions = ['North', 'South', 'East', 'West'];
            const amenities = ['Park View', 'Corner Plot', 'Wide Road', 'Garden Access'];
            
            grid.innerHTML = ''; // Clear existing plots
            
            for (let i = 1; i <= 32; i++) {
                const status = statuses[Math.floor(Math.random() * statuses.length)];
                const size = 800 + Math.floor(Math.random() * 1200);
                const price = 300000 + Math.floor(Math.random() * 700000);
                
                const plot = document.createElement('div');
                plot.className = 'plot';
                plot.innerHTML = `
                    <div class="plot-status status-${status}">${statusLabels[status]}</div>
                    <div class="plot-image">Plot Preview</div>
                    <div class="plot-info">
                        <div class="plot-number">Plot A${i}</div>
                        <div class="plot-size">${size} sq ft</div>
                        <div class="plot-price">$${price.toLocaleString()}</div>
                    </div>
                    <div class="plot-features">
                        <span class="feature-tag">${directions[Math.floor(Math.random() * directions.length)]} Facing</span>
                        <span class="feature-tag">${amenities[Math.floor(Math.random() * amenities.length)]}</span>
                    </div>
                `;
                plot.addEventListener('click', () => showPlotDetails(i, size, price, status));
                grid.appendChild(plot);
            }
        };

        const showPlotDetails = (plotId, size, price, status) => {
            const modal = document.getElementById('plot-modal');
            const detailsContainer = modal.querySelector('.plot-details');
            const galleryContainer = modal.querySelector('.plot-gallery');
            const descriptionContainer = modal.querySelector('.plot-description');
            const amenitiesContainer = modal.querySelector('.amenities');
            
            // Generate gallery
            galleryContainer.innerHTML = Array(3).fill(0).map(() => `
                <div class="gallery-img">Plot View</div>
            `).join('');
            
            // Add description
            descriptionContainer.innerHTML = `
                <h3>About This Plot</h3>
                <p>Premium ${size} sq ft plot in a prime location of Phase A. This ${status} plot offers excellent connectivity 
                and is surrounded by modern amenities. Perfect for building your dream home with ample space for gardens and parking.</p>
            `;
            
            // Generate details
            const completionPercent = Math.floor(Math.random() * 100);
            const details = {
                'Plot Number': `Plot A${plotId}`,
                'Size': `${size} sq ft`,
                'Price': `$${price.toLocaleString()}`,
                'Status': statusLabels[status],
                'Location': 'Phase A, Block 2',
                'Direction': ['North', 'South', 'East', 'West'][Math.floor(Math.random() * 4)] + ' Facing',
                'Dimensions': '40 x 80 ft',
                'Development Status': `
                    <div class="progress-bar">
                        <div class="progress-fill" style="width: ${completionPercent}%"></div>
                    </div>
                    <div style="margin-top: 5px">${completionPercent}% Complete</div>
                `
            };

            detailsContainer.innerHTML = Object.entries(details)
                .map(([label, value]) => `
                    <div class="detail-item">
                        <div class="detail-label">${label}</div>
                        <div class="detail-value">${value}</div>
                    </div>
                `).join('');
                
            // Generate amenities
            const plotAmenities = ['24/7 Security', 'Underground Utilities', 'Street Lighting', 'Landscaped Parks', 
                                 'Community Center', 'Wide Roads'];
            amenitiesContainer.innerHTML = `
                <h3 style="grid-column: 1/-1; margin-bottom: 10px;">Amenities</h3>
                ${plotAmenities.map(amenity => `
                    <div class="amenity-item">
                        <span>✓</span>
                        ${amenity}
                    </div>
                `).join('')}
            `;

            modal.style.display = 'block';
        };

        // Action button handlers
        const handleBooking = () => {
            alert('Opening booking form...');
            // Add booking form logic here
        };

        const scheduleVisit = () => {
            alert('Opening visit scheduler...');
            // Add visit scheduling logic here
        };

        const downloadBrochure = () => {
            alert('Downloading plot brochure...');
            // Add download logic here
        };

        // View toggle functionality
        document.querySelectorAll('.view-button').forEach(button => {
            button.addEventListener('click', () => {
                document.querySelectorAll('.view-button').forEach(btn => btn.classList.remove('active'));
                button.classList.add('active');
                // Add view switching logic here
            });
        });

        // Close modal
        document.querySelector('.close-modal').onclick = () => {
            document.getElementById('plot-modal').style.display = 'none';
        };

        window.onclick = (event) => {
            const modal = document.getElementById('plot-modal');
            if (event.target === modal) {
                modal.style.display = 'none';
            }
        };

        // Search functionality
        document.querySelector('.search-bar').addEventListener('input', (e) => {
            const searchTerm = e.target.value.toLowerCase();
            document.querySelectorAll('.plot').forEach(plot => {
                const plotText = plot.textContent.toLowerCase();
                plot.style.display = plotText.includes(searchTerm) ? 'flex' : 'none';
            });
        });

        // Filter functionality
        const filters = ['phase-filter', 'status-filter', 'size-filter', 'price-filter'];
        filters.forEach(filterId => {
            document.getElementById(filterId).addEventListener('change', applyFilters);
        });

        function applyFilters() {
            const selectedPhase = document.getElementById('phase-filter').value;
            const selectedStatus = document.getElementById('status-filter').value;
            const selectedSize = document.getElementById('size-filter').value;
            const selectedPrice = document.getElementById('price-filter').value;

            document.querySelectorAll('.plot').forEach(plot => {
                const plotSize = parseInt(plot.querySelector('.plot-size').textContent);
                const plotPrice = parseInt(plot.querySelector('.plot-price').textContent.replace(/[$,]/g, ''));
                const plotStatus = plot.querySelector('.plot-status').textContent.toLowerCase();

                let showPlot = true;

                if (selectedStatus && !plotStatus.includes(selectedStatus)) showPlot = false;
                if (selectedSize) {
                    if (selectedSize === 'small' && plotSize >= 1000) showPlot = false;
                    if (selectedSize === 'medium' && (plotSize < 1000 || plotSize > 2000)) showPlot = false;
                    if (selectedSize === 'large' && plotSize <= 2000) showPlot = false;
                }
                if (selectedPrice) {
                    if (selectedPrice === '0-500000' && plotPrice >= 500000) showPlot = false;
                    if (selectedPrice === '500000-1000000' && (plotPrice < 500000 || plotPrice > 1000000)) showPlot = false;
                    if (selectedPrice === '1000000+' && plotPrice <= 1000000) showPlot = false;
                }

                plot.style.display = showPlot ? 'flex' : 'none';
            });
        }

        // Initialize
        generatePlots();
    </script>
</body>
</html>
