# Storm-Shadow
Quality Code Product Of the Professional


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Neural Data Sphere - AI 3D Dashboard</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            background: linear-gradient(135deg, #0f0f23, #1a1a3a, #2d1b69);
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            overflow: hidden;
            color: white;
        }
        
        #container {
            position: relative;
            width: 100vw;
            height: 100vh;
        }
        
        #canvas {
            position: absolute;
            top: 0;
            left: 0;
            z-index: 1;
        }
        
        .ui-overlay {
            position: absolute;
            z-index: 10;
            pointer-events: none;
        }
        
        .control-panel {
            top: 20px;
            left: 20px;
            background: rgba(15, 15, 35, 0.9);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(100, 255, 218, 0.3);
            border-radius: 15px;
            padding: 20px;
            min-width: 300px;
            pointer-events: all;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
        }
        
        .stats-panel {
            top: 20px;
            right: 20px;
            background: rgba(15, 15, 35, 0.9);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 100, 100, 0.3);
            border-radius: 15px;
            padding: 20px;
            min-width: 250px;
            pointer-events: all;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
        }
        
        .title {
            font-size: 1.5rem;
            font-weight: bold;
            margin-bottom: 15px;
            background: linear-gradient(45deg, #64ffda, #ff6b6b, #4ecdc4);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }
        
        .control-group {
            margin-bottom: 15px;
        }
        
        label {
            display: block;
            font-size: 0.9rem;
            margin-bottom: 5px;
            color: #64ffda;
        }
        
        input[type="range"] {
            width: 100%;
            height: 8px;
            border-radius: 5px;
            background: rgba(100, 255, 218, 0.2);
            outline: none;
            -webkit-appearance: none;
        }
        
        input[type="range"]::-webkit-slider-thumb {
            appearance: none;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            background: #64ffda;
            cursor: pointer;
            box-shadow: 0 0 10px rgba(100, 255, 218, 0.5);
        }
        
        button {
            background: linear-gradient(45deg, #64ffda, #4ecdc4);
            border: none;
            color: #0f0f23;
            padding: 10px 20px;
            border-radius: 25px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s ease;
            margin: 5px;
        }
        
        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(100, 255, 218, 0.4);
        }
        
        .stat-item {
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
            font-size: 0.9rem;
        }
        
        .stat-value {
            color: #ff6b6b;
            font-weight: bold;
        }
        
        .neural-pattern {
            position: absolute;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            background: rgba(15, 15, 35, 0.9);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(76, 205, 196, 0.3);
            border-radius: 15px;
            padding: 15px;
            pointer-events: all;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
        }
        
        .pattern-display {
            font-family: 'Courier New', monospace;
            font-size: 0.8rem;
            color: #4ecdc4;
            line-height: 1.2;
        }
        
        @keyframes pulse {
            0%, 100% { opacity: 0.8; }
            50% { opacity: 1; }
        }
        
        .pulsing {
            animation: pulse 2s infinite;
        }
    </style>
</head>
<body>
    <div id="container">
        <canvas id="canvas"></canvas>
        
        <div class="ui-overlay control-panel">
            <div class="title">Neural Control Matrix</div>
            
            <div class="control-group">
                <label>Particle Count</label>
                <input type="range" id="particleCount" min="500" max="5000" value="2000">
            </div>
            
            <div class="control-group">
                <label>Neural Intensity</label>
                <input type="range" id="neuralIntensity" min="0.1" max="2.0" step="0.1" value="1.0">
            </div>
            
            <div class="control-group">
                <label>Gravity Strength</label>
                <input type="range" id="gravity" min="0" max="0.01" step="0.001" value="0.005">
            </div>
            
            <div class="control-group">
                <label>Connection Distance</label>
                <input type="range" id="connectionDist" min="50" max="200" value="100">
            </div>
            
            <button onclick="togglePhysics()">Toggle Physics</button>
            <button onclick="resetSimulation()">Reset Simulation</button>
            <button onclick="exportData()">Export Neural Data</button>
        </div>
        
        <div class="ui-overlay stats-panel">
            <div class="title">System Metrics</div>
            
            <div class="stat-item">
                <span>FPS:</span>
                <span class="stat-value" id="fps">60</span>
            </div>
            
            <div class="stat-item">
                <span>Active Particles:</span>
                <span class="stat-value" id="activeParticles">2000</span>
            </div>
            
            <div class="stat-item">
                <span>Neural Connections:</span>
                <span class="stat-value" id="connections">0</span>
            </div>
            
            <div class="stat-item">
                <span>Processing Power:</span>
                <span class="stat-value" id="processingPower">87%</span>
            </div>
            
            <div class="stat-item">
                <span>Memory Usage:</span>
                <span class="stat-value" id="memoryUsage">1.2 GB</span>
            </div>
            
            <div class="stat-item">
                <span>Neural Efficiency:</span>
                <span class="stat-value pulsing" id="efficiency">94.7%</span>
            </div>
        </div>
        
        <div class="ui-overlay neural-pattern">
            <div class="title">Neural Pattern Analysis</div>
            <div class="pattern-display" id="patternDisplay">
                Initializing neural matrix...<br>
                Scanning dimensional vectors...<br>
                Calibrating quantum fluctuations...
            </div>
        </div>
    </div>

    <script>
        // Global variables
        let scene, camera, renderer, particles, particleSystem;
        let connections = [];
        let physicsEnabled = true;
        let animationId;
        
        // Performance tracking
        let frameCount = 0;
        let lastTime = performance.now();
        
        // Neural network simulation
        class NeuralParticle {
            constructor(x, y, z) {
                this.position = new THREE.Vector3(x, y, z);
                this.velocity = new THREE.Vector3(
                    (Math.random() - 0.5) * 2,
                    (Math.random() - 0.5) * 2,
                    (Math.random() - 0.5) * 2
                );
                this.acceleration = new THREE.Vector3();
                this.activation = Math.random();
                this.connections = [];
                this.energy = Math.random() * 100;
                this.neuralType = Math.floor(Math.random() * 3); // 0: input, 1: hidden, 2: output
            }
            
            update() {
                if (!physicsEnabled) return;
                
                // Apply forces
                this.acceleration.multiplyScalar(0.98); // Damping
                this.velocity.add(this.acceleration);
                this.velocity.multiplyScalar(0.995); // Friction
                this.position.add(this.velocity);
                
                // Boundary constraints with elastic collision
                const boundary = 300;
                if (Math.abs(this.position.x) > boundary) {
                    this.velocity.x *= -0.8;
                    this.position.x = Math.sign(this.position.x) * boundary;
                }
                if (Math.abs(this.position.y) > boundary) {
                    this.velocity.y *= -0.8;
                    this.position.y = Math.sign(this.position.y) * boundary;
                }
                if (Math.abs(this.position.z) > boundary) {
                    this.velocity.z *= -0.8;
                    this.position.z = Math.sign(this.position.z) * boundary;
                }
                
                // Neural activation decay and regeneration
                this.activation *= 0.99;
                if (Math.random() < 0.01) {
                    this.activation = Math.min(1.0, this.activation + Math.random() * 0.3);
                }
                
                // Energy dynamics
                this.energy = Math.max(0, this.energy - 0.1 + Math.random() * 0.2);
                
                this.acceleration.set(0, 0, 0);
            }
            
            applyForce(force) {
                this.acceleration.add(force);
            }
        }
        
        // Advanced AI simulation patterns
        const neuralPatterns = [
            "█████ DEEP LEARNING MATRIX █████\n01001010 11010110 10101011\nSYNAPSE_FIRE: {HIGH_INTENSITY}\nNEURAL_PATHWAYS: OPTIMIZING...",
            
            "▓▓▓ QUANTUM NEURAL PROCESSING ▓▓▓\nQUBIT_STATE: |ψ⟩ = α|0⟩ + β|1⟩\nENTANGLEMENT: 94.7% COHERENCE\nSUPERPOSITION: ACTIVE",
            
            "░░░ MACHINE CONSCIOUSNESS ░░░\nSELF_AWARENESS: EMERGING\nCOGNITIVE_LOAD: 87.3%\nEMOTION_ENGINE: {CURIOSITY++}",
            
            "■■■ TRANSFORMER ATTENTION ■■■\nATTN_HEADS: [8x12] MATRIX\nCONTEXT_WINDOW: 2048 TOKENS\nGRADIENT_FLOW: STABLE",
            
            "▲▲▲ NEURAL ARCHITECTURE ▲▲▲\nCONV_LAYERS: 18 DEPTH\nRECURRENT_MEMORY: LSTM_CELL\nACTIVATION: SWISH_PRIME"
        ];
        
        // Initialize the 3D scene
        function init() {
            // Scene setup
            scene = new THREE.Scene();
            camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 2000);
            renderer = new THREE.WebGLRenderer({ canvas: document.getElementById('canvas'), antialias: true, alpha: true });
            renderer.setSize(window.innerWidth, window.innerHeight);
            renderer.setClearColor(0x000000, 0);
            
            // Camera position
            camera.position.set(0, 0, 500);
            
            // Create neural particle system
            createParticleSystem();
            
            // Add dynamic lighting
            const ambientLight = new THREE.AmbientLight(0x404040, 0.3);
            scene.add(ambientLight);
            
            const pointLight = new THREE.PointLight(0x64ffda, 1, 1000);
            pointLight.position.set(0, 0, 0);
            scene.add(pointLight);
            
            // Add event listeners
            setupEventListeners();
            
            // Start animation loop
            animate();
            
            // Start neural pattern updates
            setInterval(updateNeuralPattern, 3000);
            updateNeuralPattern();
        }
        
        function createParticleSystem() {
            const particleCount = parseInt(document.getElementById('particleCount').value);
            particles = [];
            
            // Create neural particles
            const geometry = new THREE.BufferGeometry();
            const positions = new Float32Array(particleCount * 3);
            const colors = new Float32Array(particleCount * 3);
            const sizes = new Float32Array(particleCount);
            
            for (let i = 0; i < particleCount; i++) {
                // Create neural particle
                const particle = new NeuralParticle(
                    (Math.random() - 0.5) * 600,
                    (Math.random() - 0.5) * 600,
                    (Math.random() - 0.5) * 600
                );
                particles.push(particle);
                
                // Set positions
                positions[i * 3] = particle.position.x;
                positions[i * 3 + 1] = particle.position.y;
                positions[i * 3 + 2] = particle.position.z;
                
                // Set colors based on neural type
                const colorIntensity = particle.activation;
                if (particle.neuralType === 0) {
                    // Input neurons - cyan
                    colors[i * 3] = 0.4 * colorIntensity;
                    colors[i * 3 + 1] = 1.0 * colorIntensity;
                    colors[i * 3 + 2] = 0.8 * colorIntensity;
                } else if (particle.neuralType === 1) {
                    // Hidden neurons - purple
                    colors[i * 3] = 0.8 * colorIntensity;
                    colors[i * 3 + 1] = 0.4 * colorIntensity;
                    colors[i * 3 + 2] = 1.0 * colorIntensity;
                } else {
                    // Output neurons - red
                    colors[i * 3] = 1.0 * colorIntensity;
                    colors[i * 3 + 1] = 0.4 * colorIntensity;
                    colors[i * 3 + 2] = 0.4 * colorIntensity;
                }
                
                sizes[i] = 2 + particle.energy * 0.1;
            }
            
            geometry.setAttribute('position', new THREE.BufferAttribute(positions, 3));
            geometry.setAttribute('color', new THREE.BufferAttribute(colors, 3));
            geometry.setAttribute('size', new THREE.BufferAttribute(sizes, 1));
            
            const material = new THREE.ShaderMaterial({
                vertexShader: `
                    attribute float size;
                    varying vec3 vColor;
                    
                    void main() {
                        vColor = color;
                        vec4 mvPosition = modelViewMatrix * vec4(position, 1.0);
                        gl_PointSize = size * (300.0 / -mvPosition.z);
                        gl_Position = projectionMatrix * mvPosition;
                    }
                `,
                fragmentShader: `
                    varying vec3 vColor;
                    
                    void main() {
                        float r = length(gl_PointCoord - vec2(0.5, 0.5));
                        if (r > 0.5) discard;
                        
                        float intensity = 1.0 - (r * 2.0);
                        gl_FragColor = vec4(vColor * intensity, intensity * 0.8);
                    }
                `,
                vertexColors: true,
                transparent: true,
                blending: THREE.AdditiveBlending
            });
            
            if (particleSystem) {
                scene.remove(particleSystem);
            }
            
            particleSystem = new THREE.Points(geometry, material);
            scene.add(particleSystem);
        }
        
        function updateParticleSystem() {
            const positions = particleSystem.geometry.attributes.position.array;
            const colors = particleSystem.geometry.attributes.color.array;
            const sizes = particleSystem.geometry.attributes.size.array;
            const intensity = parseFloat(document.getElementById('neuralIntensity').value);
            const gravityStrength = parseFloat(document.getElementById('gravity').value);
            
            // Neural network simulation
            let activeConnections = 0;
            
            for (let i = 0; i < particles.length; i++) {
                const particle = particles[i];
                
                // Apply neural forces
                if (physicsEnabled) {
                    // Gravity towards center
                    const gravity = particle.position.clone().multiplyScalar(-gravityStrength);
                    particle.applyForce(gravity);
                    
                    // Neural interaction forces
                    for (let j = i + 1; j < particles.length; j++) {
                        const other = particles[j];
                        const distance = particle.position.distanceTo(other.position);
                        const connectionDistance = parseInt(document.getElementById('connectionDist').value);
                        
                        if (distance < connectionDistance) {
                            activeConnections++;
                            
                            // Neural connection force
                            const force = other.position.clone().sub(particle.position);
                            const strength = (particle.activation * other.activation) * intensity * 0.01;
                            force.normalize().multiplyScalar(strength);
                            
                            particle.applyForce(force);
                            other.applyForce(force.multiplyScalar(-1));
                        }
                    }
                }
                
                particle.update();
                
                // Update buffer attributes
                positions[i * 3] = particle.position.x;
                positions[i * 3 + 1] = particle.position.y;
                positions[i * 3 + 2] = particle.position.z;
                
                // Update colors based on activation
                const colorIntensity = particle.activation * intensity;
                if (particle.neuralType === 0) {
                    colors[i * 3] = 0.4 * colorIntensity;
                    colors[i * 3 + 1] = 1.0 * colorIntensity;
                    colors[i * 3 + 2] = 0.8 * colorIntensity;
                } else if (particle.neuralType === 1) {
                    colors[i * 3] = 0.8 * colorIntensity;
                    colors[i * 3 + 1] = 0.4 * colorIntensity;
                    colors[i * 3 + 2] = 1.0 * colorIntensity;
                } else {
                    colors[i * 3] = 1.0 * colorIntensity;
                    colors[i * 3 + 1] = 0.4 * colorIntensity;
                    colors[i * 3 + 2] = 0.4 * colorIntensity;
                }
                
                sizes[i] = 2 + particle.energy * 0.05 * intensity;
            }
            
            particleSystem.geometry.attributes.position.needsUpdate = true;
            particleSystem.geometry.attributes.color.needsUpdate = true;
            particleSystem.geometry.attributes.size.needsUpdate = true;
            
            // Update stats
            document.getElementById('connections').textContent = activeConnections;
            document.getElementById('activeParticles').textContent = particles.length;
        }
        
        function animate() {
            animationId = requestAnimationFrame(animate);
            
            // Update particle system
            updateParticleSystem();
            
            // Camera orbit
            const time = Date.now() * 0.0001;
            camera.position.x = Math.cos(time) * 500;
            camera.position.z = Math.sin(time) * 500;
            camera.lookAt(scene.position);
            
            // Render
            renderer.render(scene, camera);
            
            // Update FPS
            frameCount++;
            if (frameCount % 60 === 0) {
                const currentTime = performance.now();
                const fps = Math.round(60000 / (currentTime - lastTime));
                document.getElementById('fps').textContent = fps;
                lastTime = currentTime;
                
                // Update other stats with simulated values
                document.getElementById('processingPower').textContent = 
                    (80 + Math.random() * 20).toFixed(1) + '%';
                document.getElementById('memoryUsage').textContent = 
                    (1.0 + Math.random() * 0.5).toFixed(1) + ' GB';
                document.getElementById('efficiency').textContent = 
                    (90 + Math.random() * 10).toFixed(1) + '%';
            }
        }
        
        function setupEventListeners() {
            // Particle count change
            document.getElementById('particleCount').addEventListener('input', function() {
                createParticleSystem();
            });
            
            // Window resize
            window.addEventListener('resize', function() {
                camera.aspect = window.innerWidth / window.innerHeight;
                camera.updateProjectionMatrix();
                renderer.setSize(window.innerWidth, window.innerHeight);
            });
            
            // Mouse interaction
            let mouseX = 0, mouseY = 0;
            document.addEventListener('mousemove', function(event) {
                mouseX = (event.clientX / window.innerWidth) * 2 - 1;
                mouseY = -(event.clientY / window.innerHeight) * 2 + 1;
                
                // Apply mouse influence to particles
                if (particles && physicsEnabled) {
                    const mouseInfluence = new THREE.Vector3(mouseX * 100, mouseY * 100, 0);
                    particles.forEach(particle => {
                        const distance = particle.position.distanceTo(mouseInfluence);
                        if (distance < 200) {
                            const force = mouseInfluence.clone().sub(particle.position);
                            force.normalize().multiplyScalar(0.02);
                            particle.applyForce(force);
                            particle.activation = Math.min(1.0, particle.activation + 0.1);
                        }
                    });
                }
            });
        }
        
        function updateNeuralPattern() {
            const patternDisplay = document.getElementById('patternDisplay');
            const randomPattern = neuralPatterns[Math.floor(Math.random() * neuralPatterns.length)];
            
            // Typewriter effect
            patternDisplay.innerHTML = '';
            let i = 0;
            const typeInterval = setInterval(() => {
                if (i < randomPattern.length) {
                    patternDisplay.innerHTML += randomPattern[i] === '\n' ? '<br>' : randomPattern[i];
                    i++;
                } else {
                    clearInterval(typeInterval);
                }
            }, 50);
        }
        
        // Control functions
        function togglePhysics() {
            physicsEnabled = !physicsEnabled;
            console.log('Physics:', physicsEnabled ? 'Enabled' : 'Disabled');
        }
        
        function resetSimulation() {
            createParticleSystem();
            console.log('Simulation reset');
        }
        
        function exportData() {
            const data = {
                timestamp: new Date().toISOString(),
                particleCount: particles.length,
                neuralConnections: document.getElementById('connections').textContent,
                efficiency: document.getElementById('efficiency').textContent,
                particles: particles.map(p => ({
                    position: [p.position.x, p.position.y, p.position.z],
                    activation: p.activation,
                    energy: p.energy,
                    type: p.neuralType
                }))
            };
            
            const blob = new Blob([JSON.stringify(data, null, 2)], {type: 'application/json'});
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = `neural_data_${Date.now()}.json`;
            a.click();
            URL.revokeObjectURL(url);
            
            console.log('Neural data exported');
        }
        
        // Initialize when page loads
        window.addEventListener('load', init);
    </script>
</body>
</html>
