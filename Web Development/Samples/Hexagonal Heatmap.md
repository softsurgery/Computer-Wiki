```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hexagonal Heatmap</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/d3@7"></script>
    <style>
        .hexagon {
            stroke: #fff;
            stroke-width: 0.5px;
            transition: all 0.3s ease;
        }
        .hexagon:hover {
            stroke-width: 2px;
            stroke: #000;
            filter: brightness(1.1);
        }
        #heatmap-container {
            background-color: #f8fafc;
        }
    </style>
</head>
<body class="bg-gray-50 min-h-screen flex flex-col items-center justify-center p-4">
    <div class="max-w-6xl w-full">
        <h1 class="text-3xl font-bold text-gray-800 mb-2 text-center">Hexagonal Heatmap</h1>
        <p class="text-gray-600 mb-8 text-center">Interactive heatmap with hexagonal grid visualization</p>
        
        <div class="bg-white rounded-xl shadow-lg p-6 mb-8">
            <div class="flex flex-wrap gap-4 mb-6 justify-center">
                <div class="flex items-center">
                    <div class="w-4 h-4 bg-blue-500 rounded-full mr-2"></div>
                    <span class="text-sm">Low intensity</span>
                </div>
                <div class="flex items-center">
                    <div class="w-4 h-4 bg-purple-500 rounded-full mr-2"></div>
                    <span class="text-sm">Medium</span>
                </div>
                <div class="flex items-center">
                    <div class="w-4 h-4 bg-red-500 rounded-full mr-2"></div>
                    <span class="text-sm">High intensity</span>
                </div>
            </div>
            
            <div id="heatmap-container" class="w-full h-auto rounded-lg overflow-hidden border border-gray-200">
                <svg id="heatmap" width="100%" height="600"></svg>
            </div>
        </div>
        
        <div class="bg-white rounded-xl shadow-lg p-6">
            <h2 class="text-xl font-semibold text-gray-800 mb-4">Heatmap Controls</h2>
            <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                <div>
                    <label class="block text-sm font-medium text-gray-700 mb-1">Grid Size</label>
                    <input type="range" id="grid-size" min="10" max="30" value="20" class="w-full h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer">
                    <div class="flex justify-between text-xs text-gray-500 mt-1">
                        <span>Small</span>
                        <span>Large</span>
                    </div>
                </div>
                <div>
                    <label class="block text-sm font-medium text-gray-700 mb-1">Data Variance</label>
                    <input type="range" id="data-variance" min="1" max="10" value="5" class="w-full h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer">
                    <div class="flex justify-between text-xs text-gray-500 mt-1">
                        <span>Uniform</span>
                        <span>Varied</span>
                    </div>
                </div>
                <div>
                    <label class="block text-sm font-medium text-gray-700 mb-1">Color Scheme</label>
                    <select id="color-scheme" class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500">
                        <option value="blue-red">Blue to Red</option>
                        <option value="green-purple">Green to Purple</option>
                        <option value="yellow-blue">Yellow to Blue</option>
                    </select>
                </div>
            </div>
            <button id="regenerate-btn" class="mt-6 w-full md:w-auto bg-indigo-600 hover:bg-indigo-700 text-white font-medium py-2 px-6 rounded-md transition duration-150 ease-in-out">
                Regenerate Heatmap
            </button>
        </div>
    </div>

    <script>
        // Add tooltip styles
        const style = document.createElement('style');
        style.textContent = `
            .hex-tooltip {
                transition: opacity 0.2s ease;
                box-shadow: 0 2px 8px rgba(0,0,0,0.1);
            }
        `;
        document.head.appendChild(style);

        document.addEventListener('DOMContentLoaded', function() {
            // Configuration
            const width = document.getElementById('heatmap').clientWidth;
            const height = 600;
            const hexRadius = 20;
            
            // Create SVG
            const svg = d3.select("#heatmap")
                .attr("viewBox", `0 0 ${width} ${height}`)
                .attr("preserveAspectRatio", "xMidYMid meet");
            
            // Hexagonal grid generator
            function generateHexGrid(radius, variance) {
                const hexWidth = radius * Math.sqrt(3);
                const hexHeight = radius * 1.5;
                
                const cols = Math.ceil(width / hexWidth) + 1;
                const rows = Math.ceil(height / hexHeight) + 1;
                
                const hexagons = [];
                
                for (let i = 0; i < rows; i++) {
                    for (let j = 0; j < cols; j++) {
                        const x = j * hexWidth + (i % 2) * hexWidth / 2;
                        const y = i * (radius * 1.5);
                        
                        if (x >= -radius && y >= -radius && x <= width + radius && y <= height + radius) {
                            const value = Math.random() * variance;
                            hexagons.push({ x, y, value });
                        }
                    }
                }
                
                return hexagons;
            }
            
            // Color scale based on value
            function getColorScale(scheme) {
                switch(scheme) {
                    case 'blue-red':
                        return d3.scaleLinear()
                            .domain([0, 0.5, 1])
                            .range(['#3b82f6', '#a855f7', '#ef4444']);
                    case 'green-purple':
                        return d3.scaleLinear()
                            .domain([0, 0.5, 1])
                            .range(['#10b981', '#8b5cf6', '#ec4899']);
                    case 'yellow-blue':
                        return d3.scaleLinear()
                            .domain([0, 0.5, 1])
                            .range(['#f59e0b', '#3b82f6', '#1e40af']);
                    default:
                        return d3.scaleLinear()
                            .domain([0, 0.5, 1])
                            .range(['#3b82f6', '#a855f7', '#ef4444']);
                }
            }
            
            // Create hexagon path
            function hexagonPath(radius) {
                const points = [];
                for (let i = 0; i < 6; i++) {
                    const angle = (i * 2 * Math.PI / 6) - Math.PI / 6;
                    points.push([
                        radius * Math.cos(angle),
                        radius * Math.sin(angle)
                    ]);
                }
                return "M" + points.join("L") + "Z";
            }
            
            // Draw heatmap
            function drawHeatmap(radius, variance, colorScheme) {
                svg.selectAll("*").remove();
                
                const hexagons = generateHexGrid(radius, variance);
                const colorScale = getColorScale(colorScheme);
                
                // Create hexagon groups
                const hexGroups = svg.selectAll(".hex-group")
                    .data(hexagons)
                    .enter()
                    .append("g")
                    .attr("class", "hex-group")
                    .attr("transform", d => `translate(${d.x}, ${d.y})`);
                
                // Draw hexagons
                hexGroups.append("path")
                    .attr("class", "hexagon")
                    .attr("d", hexagonPath(radius))
                    .attr("fill", d => colorScale(d.value))
                    .attr("opacity", 0.9)
                    .on("mouseover", function(event, d) {
                        d3.select(this)
                            .attr("opacity", 1)
                            .attr("stroke", "#000");
                        
                        // Remove any existing tooltips first
                        d3.selectAll(".hex-tooltip").remove();
                        
                        // Show tooltip with unique class
                        const tooltip = d3.select("body").append("div")
                            .attr("class", "hex-tooltip absolute bg-gray-800 text-white text-xs px-2 py-1 rounded-md pointer-events-none")
                            .style("left", (event.pageX + 10) + "px")
                            .style("top", (event.pageY - 15) + "px")
                            .style("z-index", "100")
                            .text(`Value: ${d.value.toFixed(2)}`);
                    })
                    .on("mouseout", function() {
                        d3.select(this)
                            .attr("opacity", 0.9)
                            .attr("stroke", "#fff");
                        // Remove all tooltips with our class
                        d3.selectAll(".hex-tooltip").remove();
                    });
            }
            
            // Initial draw
            drawHeatmap(hexRadius, 5, 'blue-red');
            
            // Event listeners for controls
            document.getElementById('regenerate-btn').addEventListener('click', function() {
                const radius = parseInt(document.getElementById('grid-size').value);
                const variance = parseInt(document.getElementById('data-variance').value);
                const colorScheme = document.getElementById('color-scheme').value;
                drawHeatmap(radius, variance, colorScheme);
            });
            
            document.getElementById('grid-size').addEventListener('input', function() {
                const radius = parseInt(this.value);
                const variance = parseInt(document.getElementById('data-variance').value);
                const colorScheme = document.getElementById('color-scheme').value;
                drawHeatmap(radius, variance, colorScheme);
            });
            
            document.getElementById('data-variance').addEventListener('input', function() {
                const radius = parseInt(document.getElementById('grid-size').value);
                const variance = parseInt(this.value);
                const colorScheme = document.getElementById('color-scheme').value;
                drawHeatmap(radius, variance, colorScheme);
            });
            
            document.getElementById('color-scheme').addEventListener('change', function() {
                const radius = parseInt(document.getElementById('grid-size').value);
                const variance = parseInt(document.getElementById('data-variance').value);
                const colorScheme = this.value;
                drawHeatmap(radius, variance, colorScheme);
            });
        });
    </script>
</body>
</html>

```