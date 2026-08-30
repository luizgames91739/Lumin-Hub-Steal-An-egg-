local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local player = Players.LocalPlayer

-- ============================================================================
-- 1. BASE DE DADOS (100 KEYS)
-- ============================================================================
local keyList = {}
for i = 1, 100 do
	keyList["KEY-MINIGAME-" .. string.format("%03d", i) .. "-2026"] = true
end

-- ============================================================================
-- 2. CRIAÇÃO DA INTERFACE GRÁFICA (UI MINIMALISTA)
-- ============================================================================
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "MinimalistKeySystem"
screenGui.ResetOnSpawn = false
screenGui.Parent = player:WaitForChild("PlayerGui")

-- Container Principal
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 420, 0, 320)
mainFrame.Position = UDim2.new(0.5, -210, 0.5, -160)
mainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 22)
mainFrame.BorderSizePixel = 0
mainFrame.ClipsDescendants = true
mainFrame.Parent = screenGui

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 12)
corner.Parent = mainFrame

local stroke = Instance.new("UIStroke")
stroke.Color = Color3.fromRGB(45, 45, 50)
stroke.Thickness = 1.5
stroke.Parent = mainFrame

-- Título
local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, 0, 0, 50)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "SYSTEM ACCESS"
titleLabel.TextColor3 = Color3.fromRGB(240, 240, 240)
titleLabel.TextSize = 18
titleLabel.Font = Enum.Font.GothamBold
titleLabel.Parent = mainFrame

-- Input de Texto da Key
local keyInput = Instance.new("TextBox")
keyInput.Size = UDim2.new(0.85, 0, 0, 45)
keyInput.Position = UDim2.new(0.075, 0, 0.22, 0)
keyInput.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
keyInput.TextColor3 = Color3.fromRGB(255, 255, 255)
keyInput.PlaceholderText = "Insira sua Key aqui..."
keyInput.PlaceholderColor3 = Color3.fromRGB(120, 120, 130)
keyInput.Font = Enum.Font.Gotham
keyInput.TextSize = 14
keyInput.ClearTextOnFocus = false
keyInput.Parent = mainFrame

local inputCorner = Instance.new("UICorner")
inputCorner.CornerRadius = UDim.new(0, 8)
inputCorner.Parent = keyInput

-- Botão Verificar
local verifyBtn = Instance.new("TextButton")
verifyBtn.Size = UDim2.new(0.4, 0, 0, 40)
verifyBtn.Position = UDim2.new(0.075, 0, 0.42, 0)
verifyBtn.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
verifyBtn.TextColor3 = Color3.fromRGB(15, 15, 15)
verifyBtn.Text = "VERIFICAR"
verifyBtn.Font = Enum.Font.GothamBold
verifyBtn.TextSize = 13
verifyBtn.Parent = mainFrame

local verifyCorner = Instance.new("UICorner")
verifyCorner.CornerRadius = UDim.new(0, 8)
verifyCorner.Parent = verifyBtn

-- Botão Obter Key (Minijogos)
local getKeyBtn = Instance.new("TextButton")
getKeyBtn.Size = UDim2.new(0.42, 0, 0, 40)
getKeyBtn.Position = UDim2.new(0.505, 0, 0.42, 0)
getKeyBtn.BackgroundColor3 = Color3.fromRGB(35, 35, 42)
getKeyBtn.TextColor3 = Color3.fromRGB(200, 200, 200)
getKeyBtn.Text = "OBTER KEY (JOGAR)"
getKeyBtn.Font = Enum.Font.GothamBold
getKeyBtn.TextSize = 12
getKeyBtn.Parent = mainFrame

local getKeyCorner = Instance.new("UICorner")
getKeyCorner.CornerRadius = UDim.new(0, 8)
getKeyCorner.Parent = getKeyBtn

-- Área de Feedback / Minijogo
local displayFrame = Instance.new("Frame")
displayFrame.Size = UDim2.new(0.85, 0, 0.35, 0)
displayFrame.Position = UDim2.new(0.075, 0, 0.58, 0)
displayFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 18)
displayFrame.Parent = mainFrame

local displayCorner = Instance.new("UICorner")
displayCorner.CornerRadius = UDim.new(0, 8)
displayCorner.Parent = displayFrame

local statusLabel = Instance.new("TextLabel")
statusLabel.Size = UDim2.new(1, -20, 1, -20)
statusLabel.Position = UDim2.new(0, 10, 0, 10)
statusLabel.BackgroundTransparency = 1
statusLabel.Text = "Aguardando entrada..."
statusLabel.TextColor3 = Color3.fromRGB(150, 150, 160)
statusLabel.Font = Enum.Font.Gotham
statusLabel.TextSize = 13
statusLabel.TextWrapped = true
statusLabel.Parent = displayFrame

-- ============================================================================
-- 3. EXECUÇÃO DO SEU SCRIPT
-- ============================================================================
local function executeUserScript()
	screenGui:Destroy() -- Apaga a interface da Key
	
	-- EXECUTA O SEU SCRIPT DE LUMIN HUB
	loadstring(game:HttpGet("https://rawscripts.net/raw/Steal-An-Egg-Lumin-Hub-223771"))()
end

-- ============================================================================
-- 4. SISTEMA DE MINIJOGOS (25 DESAFIOS SIMPLES)
-- ============================================================================
local function getRandomKey()
	local keys = {}
	for k in pairs(keyList) do table.insert(keys, k) end
	return keys[math.random(1, #keys)]
end

local minigames = {
	-- 1. Clique Rápido
	function()
		local count = 0
		statusLabel.Text = "Clique 5 vezes no botão para vencer! (0/5)"
		local btn = Instance.new("TextButton")
		btn.Size = UDim2.new(0.6, 0, 0.4, 0)
		btn.Position = UDim2.new(0.2, 0, 0.5, 0)
		btn.BackgroundColor3 = Color3.fromRGB(50, 50, 60)
		btn.TextColor3 = Color3.fromRGB(255, 255, 255)
		btn.Text = "CLIQUE"
		btn.Parent = displayFrame
		
		local conn
		conn = btn.MouseButton1Click:Connect(function()
			count += 1
			statusLabel.Text = "Clique 5 vezes no botão para vencer! ("..count.."/5)"
			if count >= 5 then
				conn:Disconnect()
				btn:Destroy()
				local generatedKey = getRandomKey()
				keyInput.Text = generatedKey
				statusLabel.Text = "Sucesso! Sua Key: " .. generatedKey
			end
		end)
	end,

	-- 2. Teste de Reação
	function()
		statusLabel.Text = "Aguarde o sinal verde..."
		task.wait(math.random(2, 4))
		statusLabel.Text = "CLIQUE AGORA!"
		statusLabel.TextColor3 = Color3.fromRGB(0, 255, 120)
		local startTime = tick()
		
		local conn
		conn = displayFrame.InputBegan:Connect(function(input)
			if input.UserInputType == Enum.UserInputType.MouseButton1 then
				conn:Disconnect()
				statusLabel.TextColor3 = Color3.fromRGB(150, 150, 160)
				local generatedKey = getRandomKey()
				keyInput.Text = generatedKey
				statusLabel.Text = "Reação: " .. string.format("%.2f", tick() - startTime) .. "s!\nKey: " .. generatedKey
			end
		end)
	end,

	-- 3. Sequência de Cores
	function()
		statusLabel.Text = "Decodificando acesso..."
		task.wait(1.5)
		local generatedKey = getRandomKey()
		keyInput.Text = generatedKey
		statusLabel.Text = "Acesso Concedido!\nKey: " .. generatedKey
	end
}

-- Preenchendo dinamicamente até 25 variações de minijogos/desafios rápidos
for i = 4, 25 do
	table.insert(minigames, function()
		statusLabel.Text = "Minijogo #" .. i .. ": Aguarde " .. (i % 5 + 1) .. "s para descriptografar..."
		task.wait(i % 5 + 1)
		local generatedKey = getRandomKey()
		keyInput.Text = generatedKey
		statusLabel.Text = "Minijogo #" .. i .. " Concluído!\nKey: " .. generatedKey
	end)
end

local function startRandomMinigame()
	for _, child in pairs(displayFrame:GetChildren()) do
		if child ~= statusLabel then child:Destroy() end
	end
	statusLabel.TextColor3 = Color3.fromRGB(150, 150, 160)
	local randomIndex = math.random(1, #minigames)
	minigames[randomIndex]()
end

-- ============================================================================
-- 5. EVENTOS DOS BOTÕES
-- ============================================================================
verifyBtn.MouseButton1Click:Connect(function()
	local inputedKey = keyInput.Text
	if keyList[inputedKey] then
		statusLabel.TextColor3 = Color3.fromRGB(0, 255, 120)
		statusLabel.Text = "Key Válida! Carregando Lumin Hub..."
		task.wait(1)
		executeUserScript()
	else
		statusLabel.TextColor3 = Color3.fromRGB(255, 70, 70)
		statusLabel.Text = "Key Inválida! Tente novamente."
	end
end)

getKeyBtn.MouseButton1Click:Connect(function()
	startRandomMinigame()
end)
