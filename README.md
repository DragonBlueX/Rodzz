-- HUB COMPLETO - Roblox Performance + Visual + FOV
local player = game:GetService("Players").LocalPlayer
local Lighting = game:GetService("Lighting")
local RunService = game:GetService("RunService")

-- Interface principal
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "FPSVisualHub"
gui.ResetOnSpawn = false

-- Janela do Hub
local mainFrame = Instance.new("Frame", gui)
mainFrame.Size = UDim2.new(0, 230, 0, 230)
mainFrame.Position = UDim2.new(0, 50, 0, 50)
mainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
mainFrame.BorderSizePixel = 0
mainFrame.Active = true
mainFrame.Draggable = true

-- Título
local title = Instance.new("TextLabel", mainFrame)
title.Size = UDim2.new(1, 0, 0, 30)
title.BackgroundTransparency = 1
title.Text = "Visual & FPS Hub"
title.Font = Enum.Font.SourceSansBold
title.TextSize = 20
title.TextColor3 = Color3.fromRGB(255, 255, 255)

-- Função Shader Realista
local function ativarShader()
	for _, v in pairs(Lighting:GetChildren()) do
		if v:IsA("BloomEffect") or v:IsA("ColorCorrectionEffect") or v:IsA("SunRaysEffect") or v:IsA("DepthOfFieldEffect") then
			v:Destroy()
		end
	end

	local bloom = Instance.new("BloomEffect", Lighting)
	bloom.Intensity = 1.2
	bloom.Size = 64
	bloom.Threshold = 0.9

	local color = Instance.new("ColorCorrectionEffect", Lighting)
	color.TintColor = Color3.fromRGB(255, 210, 190)
	color.Saturation = 0.15
	color.Contrast = 0.12
	color.Brightness = 0.03

	local rays = Instance.new("SunRaysEffect", Lighting)
	rays.Intensity = 0.08
	rays.Spread = 0.5

	local depth = Instance.new("DepthOfFieldEffect", Lighting)
	depth.FarIntensity = 0.1
	depth.FocusDistance = 25
	depth.InFocusRadius = 20
	depth.NearIntensity = 0.05

	Lighting.ClockTime = 14
	Lighting.Brightness = 2
	Lighting.ExposureCompensation = 0.5
	Lighting.GlobalShadows = false
end

-- Botão Shader
local shaderBtn = Instance.new("TextButton", mainFrame)
shaderBtn.Size = UDim2.new(1, -20, 0, 35)
shaderBtn.Position = UDim2.new(0, 10, 0, 40)
shaderBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
shaderBtn.Text = "Ativar Visual Realista"
shaderBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
shaderBtn.Font = Enum.Font.SourceSans
shaderBtn.TextSize = 16
shaderBtn.MouseButton1Click:Connect(ativarShader)

-- Botão Boost FPS
local fpsBtn = Instance.new("TextButton", mainFrame)
fpsBtn.Size = UDim2.new(1, -20, 0, 35)
fpsBtn.Position = UDim2.new(0, 10, 0, 80)
fpsBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
fpsBtn.Text = "Boostar FPS"
fpsBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
fpsBtn.Font = Enum.Font.SourceSans
fpsBtn.TextSize = 16

fpsBtn.MouseButton1Click:Connect(function()
	for _, v in pairs(game:GetDescendants()) do
		if v:IsA("ParticleEmitter") or v:IsA("Trail") then
			v.Enabled = false
		end
	end
	settings().Rendering.QualityLevel = Enum.QualityLevel.Level01
end)

-- FPS Counter
local fpsCounter = Instance.new("TextLabel", gui)
fpsCounter.Size = UDim2.new(0, 160, 0, 30)
fpsCounter.Position = UDim2.new(0, 50, 0, 300)
fpsCounter.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
fpsCounter.TextColor3 = Color3.fromRGB(0, 255, 0)
fpsCounter.Font = Enum.Font.SourceSansBold
fpsCounter.TextSize = 16
fpsCounter.Text = "FPS: ..."
fpsCounter.Draggable = true

local lastUpdate = tick()
local frames = 0
RunService.RenderStepped:Connect(function()
	frames += 1
	if tick() - lastUpdate >= 1 then
		fpsCounter.Text = "FPS: " .. frames
		frames = 0
		lastUpdate = tick()
	end
end)

-- FOV fixo no centro
local fovCircle = Drawing.new("Circle")
fovCircle.Color = Color3.fromRGB(0, 170, 255)
fovCircle.Thickness = 2
fovCircle.Radius = 120
fovCircle.NumSides = 100
fovCircle.Filled = false
fovCircle.Transparency = 1
fovCircle.Visible = true

local fovAtivo = true

RunService.RenderStepped:Connect(function()
	if fovAtivo then
		local res = workspace.CurrentCamera.ViewportSize
		fovCircle.Position = Vector2.new(res.X / 2, res.Y / 2)
	end
end)

-- Botão FOV
local fovBtn = Instance.new("TextButton", mainFrame)
fovBtn.Size = UDim2.new(1, -20, 0, 35)
fovBtn.Position = UDim2.new(0, 10, 0, 120)
fovBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
fovBtn.Text = "Alternar FOV Central"
fovBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
fovBtn.Font = Enum.Font.SourceSans
fovBtn.TextSize = 16

fovBtn.MouseButton1Click:Connect(function()
	fovAtivo = not fovAtivo
	fovCircle.Visible = fovAtivo
end)
