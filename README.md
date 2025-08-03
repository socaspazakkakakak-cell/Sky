# Sky
Velocidade
local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local player = Players.LocalPlayer

local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")

local currentSpeed = 16
local safeSpeed = 30 -- velocidade "segura" padrão

humanoid.WalkSpeed = currentSpeed

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "SpeedGui"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = player:WaitForChild("PlayerGui")

local Frame = Instance.new("Frame")
Frame.Size = UDim2.new(0, 250, 0, 120)
Frame.Position = UDim2.new(0, 20, 0, 100)
Frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Frame.BorderSizePixel = 0
Frame.Visible = true
Frame.Parent = ScreenGui

local CloseButton = Instance.new("TextButton")
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Position = UDim2.new(1, -35, 0, 5)
CloseButton.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
CloseButton.Text = "X"
CloseButton.TextColor3 = Color3.new(1, 1, 1)
CloseButton.Font = Enum.Font.SourceSansBold
CloseButton.TextSize = 18
CloseButton.Parent = Frame

CloseButton.MouseButton1Click:Connect(function()
	Frame.Visible = false
end)

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, -40, 0, 30)
Title.Position = UDim2.new(0, 10, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "Velocidade do Jogador"
Title.TextColor3 = Color3.new(1, 1, 1)
Title.Font = Enum.Font.SourceSansBold
Title.TextSize = 18
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = Frame

local SpeedButton = Instance.new("TextButton")
SpeedButton.Size = UDim2.new(1, -20, 0, 40)
SpeedButton.Position = UDim2.new(0, 10, 0, 50)
SpeedButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
SpeedButton.Text = "Clique para definir (atual: 16)"
SpeedButton.TextColor3 = Color3.new(1, 1, 1)
SpeedButton.Font = Enum.Font.SourceSans
SpeedButton.TextSize = 16
SpeedButton.Parent = Frame

local function setSpeed(value)
	if value < 16 then
		value = 16 -- nunca menos que o normal
	elseif value > 40 then
		value = 40 -- limite seguro para disfarçar
	end
	currentSpeed = value
	humanoid.WalkSpeed = currentSpeed
	SpeedButton.Text = "Clique para definir (atual: " .. currentSpeed .. ")"
end

SpeedButton.MouseButton1Click:Connect(function()
	local input = tonumber(game:GetService("StarterGui"):PromptInput("Digite a velocidade (16 a 40)"))
	if input and typeof(input) == "number" then
		setSpeed(input)
	end
end)

player.CharacterAdded:Connect(function(char)
	humanoid = char:WaitForChild("Humanoid")
	-- Mantém a velocidade configurada
	humanoid.WalkSpeed = currentSpeed
end)

local UIS = game:GetService("UserInputService")
UIS.InputBegan:Connect(function(input, gameProcessed)
	if not gameProcessed and input.KeyCode == Enum.KeyCode.V then
		Frame.Visible = not Frame.Visible
	end
end)

-- Reseta velocidade ao fechar o jogo
game:BindToClose(function()
	if humanoid then
		humanoid.WalkSpeed = 16
	end
end)

-- Inicializa velocidade padrão
setSpeed(16)
