local player = game.Players.LocalPlayer

local gui = Instance.new("ScreenGui")
gui.Name = "H4XMenu"
gui.ResetOnSpawn = false
gui.Enabled = true
gui.Parent = player:WaitForChild("PlayerGui")

-- ===== BOLINHA FLUTUANTE =====
local bola = Instance.new("ImageButton")
bola.Size = UDim2.new(0, 55, 0, 55)
bola.Position = UDim2.new(0, 20, 0.5, -27)
bola.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
bola.Image = "rbxassetid://133511512953823"
bola.ScaleType = Enum.ScaleType.Fit
bola.Parent = gui
Instance.new("UICorner", bola).CornerRadius = UDim.new(1, 0)

local bolaStroke = Instance.new("UIStroke", bola)
bolaStroke.Color = Color3.fromRGB(200, 20, 20)
bolaStroke.Thickness = 2

-- ===== FRAME PRINCIPAL DO MENU =====
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 340, 0, 300)
frame.Position = UDim2.new(0, 90, 0.5, -150)
frame.BackgroundColor3 = Color3.fromRGB(15, 15, 20)
frame.Visible = false
frame.Parent = gui
Instance.new("UICorner", frame).CornerRadius = UDim.new(0, 10)

local titleBar = Instance.new("Frame")
titleBar.Size = UDim2.new(1, 0, 0, 36)
titleBar.BackgroundColor3 = Color3.fromRGB(160, 40, 220)
titleBar.Parent = frame
Instance.new("UICorner", titleBar).CornerRadius = UDim.new(0, 10)

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, -40, 1, 0)
title.Position = UDim2.new(0, 10, 0, 0)
title.BackgroundTransparency = 1
title.Text = "MENU OTIMIZAÇÃO"
title.Font = Enum.Font.GothamBold
title.TextSize = 16
title.TextColor3 = Color3.new(1,1,1)
title.TextXAlignment = Enum.TextXAlignment.Left
title.Parent = titleBar

local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 30, 0, 30)
closeBtn.Position = UDim2.new(1, -33, 0, 3)
closeBtn.BackgroundTransparency = 1
closeBtn.Text = "X"
closeBtn.TextColor3 = Color3.new(1,1,1)
closeBtn.Font = Enum.Font.GothamBold
closeBtn.TextSize = 16
closeBtn.Parent = titleBar
closeBtn.MouseButton1Click:Connect(function() frame.Visible = false end)

-- ===== ARRASTAR A BOLINHA (e o menu junto, se estiver aberto) =====
local UIS = game:GetService("UserInputService")
local arrastandoBola, offsetBola, moveuBola, offsetFrameRelativo

bola.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        arrastandoBola = true
        moveuBola = false
        offsetBola = input.Position - bola.AbsolutePosition
        offsetFrameRelativo = frame.AbsolutePosition - bola.AbsolutePosition
    end
end)
bola.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        arrastandoBola = false
    end
end)
UIS.InputChanged:Connect(function(input)
    if arrastandoBola and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        moveuBola = true
        local novaPosBola = Vector2.new(input.Position.X - offsetBola.X, input.Position.Y - offsetBola.Y)
        bola.Position = UDim2.new(0, novaPosBola.X, 0, novaPosBola.Y)

        if frame.Visible then
            frame.Position = UDim2.new(0, novaPosBola.X + offsetFrameRelativo.X, 0, novaPosBola.Y + offsetFrameRelativo.Y)
        end
    end
end)

bola.MouseButton1Click:Connect(function()
    if not moveuBola then
        frame.Visible = not frame.Visible
    end
end)

-- ===== ARRASTAR O MENU PELA BARRA DE CIMA (continua funcionando também) =====
local arrastandoMenu, offsetMenu
titleBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        arrastandoMenu = true
        offsetMenu = input.Position - frame.AbsolutePosition
    end
end)
titleBar.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        arrastandoMenu = false
    end
end)
UIS.InputChanged:Connect(function(input)
    if arrastandoMenu and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        frame.Position = UDim2.new(0, input.Position.X - offsetMenu.X, 0, input.Position.Y - offsetMenu.Y)
    end
end)

-- ===== ABAS =====
local tabBar = Instance.new("Frame")
tabBar.Size = UDim2.new(1, 0, 0, 34)
tabBar.Position = UDim2.new(0, 0, 0, 36)
tabBar.BackgroundTransparency = 1
tabBar.Parent = frame

local abas = {"Gráficos", "Iluminação", "Info"}
local tabButtons = {}
local paginas = {}

for i, nome in ipairs(abas) do
    local tabBtn = Instance.new("TextButton")
    tabBtn.Size = UDim2.new(0, 100, 1, 0)
    tabBtn.Position = UDim2.new(0, (i-1)*105 + 5, 0, 0)
    tabBtn.BackgroundTransparency = 1
    tabBtn.Text = nome
    tabBtn.Font = Enum.Font.GothamBold
    tabBtn.TextSize = 14
    tabBtn.TextColor3 = i == 1 and Color3.fromRGB(200, 100, 255) or Color3.fromRGB(150, 150, 150)
    tabBtn.Parent = tabBar
    tabButtons[nome] = tabBtn

    local pagina = Instance.new("Frame")
    pagina.Size = UDim2.new(1, -20, 1, -80)
    pagina.Position = UDim2.new(0, 10, 0, 78)
    pagina.BackgroundTransparency = 1
    pagina.Visible = i == 1
    pagina.Parent = frame
    paginas[nome] = pagina

    tabBtn.MouseButton1Click:Connect(function()
        for n, p in pairs(paginas) do p.Visible = (n == nome) end
        for n, b in pairs(tabButtons) do
            b.TextColor3 = (n == nome) and Color3.fromRGB(200, 100, 255) or Color3.fromRGB(150, 150, 150)
        end
    end)
end

local linha = Instance.new("Frame")
linha.Size = UDim2.new(1, 0, 0, 1)
linha.Position = UDim2.new(0, 0, 0, 70)
linha.BackgroundColor3 = Color3.fromRGB(160, 40, 220)
linha.Parent = frame

-- ===== CHECKBOX =====
local function criarCheckbox(pai, texto, yPos, callback)
    local box = Instance.new("Frame")
    box.Size = UDim2.new(0, 24, 0, 24)
    box.Position = UDim2.new(0, 0, 0, yPos)
    box.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
    box.Parent = pai
    Instance.new("UICorner", box).CornerRadius = UDim.new(0, 5)

    local check = Instance.new("TextButton")
    check.Size = UDim2.new(1, 0, 1, 0)
    check.BackgroundTransparency = 1
    check.Text = ""
    check.Parent = box

    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, -34, 0, 24)
    label.Position = UDim2.new(0, 34, 0, yPos)
    label.BackgroundTransparency = 1
    label.Text = texto
    label.Font = Enum.Font.Gotham
    label.TextSize = 15
    label.TextColor3 = Color3.new(1,1,1)
    label.TextXAlignment = Enum.TextXAlignment.Left
    label.Parent = pai

    local ativo = false
    check.MouseButton1Click:Connect(function()
        ativo = not ativo
        box.BackgroundColor3 = ativo and Color3.fromRGB(160, 40, 220) or Color3.fromRGB(30, 30, 35)
        callback(ativo)
    end)
end

-- ===== ABA GRÁFICOS =====
local originalMaterial = {}
local conexaoTextura

criarCheckbox(paginas["Gráficos"], "Remover Texturas", 0, function(ativo)
    if ativo then
        for _, obj in pairs(workspace:GetDescendants()) do
            if obj:IsA("BasePart") then
                originalMaterial[obj] = obj.Material
                obj.Material = Enum.Material.SmoothPlastic
            elseif obj:IsA("Decal") or obj:IsA("Texture") then
                obj.Transparency = 1
            end
        end
        conexaoTextura = workspace.DescendantAdded:Connect(function(obj)
            if obj:IsA("BasePart") then
                originalMaterial[obj] = obj.Material
                obj.Material = Enum.Material.SmoothPlastic
            elseif obj:IsA("Decal") or obj:IsA("Texture") then
                obj.Transparency = 1
            end
        end)
    else
        if conexaoTextura then conexaoTextura:Disconnect() end
        for obj, mat in pairs(originalMaterial) do
            if obj and obj.Parent then obj.Material = mat end
        end
    end
end)

criarCheckbox(paginas["Gráficos"], "Qualidade Gráfica Mínima", 40, function(ativo)
    local Settings = UserSettings():GetService("UserGameSettings")
    Settings.SavedQualityLevel = ativo and Enum.SavingQuality.QualityLevel1 or Enum.SavingQuality.Automatic
end)

-- ===== ABA ILUMINAÇÃO =====
criarCheckbox(paginas["Iluminação"], "Desativar Sombras", 0, function(ativo)
    game.Lighting.GlobalShadows = not ativo
end)

criarCheckbox(paginas["Iluminação"], "Reduzir Névoa (Fog)", 40, function(ativo)
    game.Lighting.FogEnd = ativo and 500 or 100000
end)

-- ===== ABA INFO =====
local info = Instance.new("TextLabel")
info.Size = UDim2.new(1, 0, 1, 0)
info.BackgroundTransparency = 1
info.Text = "By skaksksk_68/parette blitz curler"
info.Font = Enum.Font.Gotham
info.TextSize = 14
info.TextColor3 = Color3.fromRGB(200, 200, 200)
info.TextWrapped = true
info.TextYAlignment = Enum.TextYAlignment.Top
info.Parent = paginas["Info"]
