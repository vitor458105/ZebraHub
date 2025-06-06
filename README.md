-- ZebraHub | Grow a Garden Script
-- Key System
local key = "Brazil"
local userInput = tostring(game:GetService("Players").LocalPlayer:WaitForChild("PlayerGui"):WaitForChild("ScreenGui"):WaitForChild("KeyInput").Text)

if userInput ~= key then
    warn("Chave incorreta.")
    return
end

-- UI Library (Simples)
local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/bloodball/-back-ups-for-libs/main/venus"))()
local win = library:CreateWindow("ZebraHub - Grow a Garden")

-- Variáveis
local player = game.Players.LocalPlayer
local garden = workspace:FindFirstChild("Garden") or workspace
local fruitsFolder = garden:FindFirstChild("Fruits")
local petsFolder = player:WaitForChild("Pets")
local seedsFolder = game:GetService("ReplicatedStorage"):WaitForChild("Seeds")

-- Funções auxiliares
local function clickFruit(fruit)
    firetouchinterest(player.Character.HumanoidRootPart, fruit, 0)
    wait(0.1)
    firetouchinterest(player.Character.HumanoidRootPart, fruit, 1)
end

local function collectFruits()
    for _, fruit in pairs(fruitsFolder:GetChildren()) do
        if fruit:IsA("BasePart") then
            clickFruit(fruit)
        end
    end
end

local function dupePets()
    local pets = petsFolder:GetChildren()
    for _, pet in pairs(pets) do
        local clone = pet:Clone()
        clone.Parent = petsFolder
    end
end

local function dupeFruits()
    local backpack = player:WaitForChild("Backpack")
    for _, item in pairs(backpack:GetChildren()) do
        if item.Name:lower():find("fruit") then
            local clone = item:Clone()
            clone.Parent = backpack
        end
    end
end

local function sellFruits()
    local sellEvent = game:GetService("ReplicatedStorage"):FindFirstChild("SellFruits")
    if sellEvent then
        sellEvent:FireServer()
    end
end

local function buySeeds()
    local buyEvent = game:GetService("ReplicatedStorage"):FindFirstChild("BuySeed")
    if buyEvent then
        for i = 1, 5 do
            buyEvent:FireServer("BasicSeed")
            wait(0.2)
        end
    end
end

-- GUI Tabs
local mainTab = win:CreateTab("Principal")
mainTab:CreateToggle("Auto Coletar Frutas", function(state)
    _G.autoCollect = state
    while _G.autoCollect do
        collectFruits()
        wait(1)
    end
end)

mainTab:CreateToggle("Auto Vender Frutas", function(state)
    _G.autoSell = state
    while _G.autoSell do
        sellFruits()
        wait(3)
    end
end)

mainTab:CreateToggle("Auto Comprar Sementes", function(state)
    _G.autoBuy = state
    while _G.autoBuy do
        buySeeds()
        wait(5)
    end
end)

local exploitTab = win:CreateTab("Exploit")
exploitTab:CreateButton("Dupe Pets", dupePets)
exploitTab:CreateButton("Dupe Frutas", dupeFruits)

-- Mensagem final
print("ZebraHub carregado com sucesso!")
