[Uploading script1.lua.txt…]()
# MyRobloxScripts1-- ====================================================
-- ТЕХНІЧНА ОСНОВА ТА СУЧАСНИЙ ДИЗАЙН ІНТЕРФЕЙСУ
-- ====================================================
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local CoreGui = game:GetService("CoreGui")

local lp = Players.LocalPlayer
local W = workspace
local R = RunService
local U = UserInputService

-- Перевірка та очищення старого GUI
local targetParent = CoreGui or lp:FindFirstChildOfClass("PlayerGui")
if targetParent:FindFirstChild("PremiumSliderGui") then
    targetParent["PremiumSliderGui"]:Destroy()
end

local mainGui = Instance.new("ScreenGui")
mainGui.Name = "PremiumSliderGui"
mainGui.ResetOnSpawn = false
mainGui.Parent = targetParent

-- КРАСИВЕ ГОЛОВНЕ ВІКНО
local pF = Instance.new("Frame", mainGui)
pF.Name = "MainFrame"
pF.Size = UDim2.new(0, 280, 0, 320)
pF.Position = UDim2.new(0.5, -140, 0.4, -160)
pF.BackgroundColor3 = Color3.fromRGB(18, 18, 24)
pF.BorderSizePixel = 0
pF.Active = true

-- Закруглення кутів для головного вікна
local mainCorner = Instance.new("UICorner", pF)
mainCorner.CornerRadius = UDim.new(0, 10)

-- НАДІЙНА ФУНКЦІЯ ПЕРЕТЯГУВАННЯ МЕНЮ
local function makeElementDraggable(obj)
    local dragging = false
    local dragInput, dragStart, startPos

    obj.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = obj.Position
            
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)

    obj.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            dragInput = input
        end
    end)

    U.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            local delta = input.Position - dragStart
            obj.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)
end

makeElementDraggable(pF)

-- Красива неонова смужка-заголовок зверху вікна
local topLine = Instance.new("Frame", pF)
topLine.Size = UDim2.new(1, 0, 0, 4)
topLine.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
topLine.BorderSizePixel = 0
Instance.new("UICorner", topLine).CornerRadius = UDim.new(0, 10)

-- Текст заголовка меню
local menuTitle = Instance.new("TextLabel", pF)
menuTitle.Size = UDim2.new(0, 150, 0, 25)
menuTitle.Position = UDim2.new(0, 12, 0, 4)
menuTitle.BackgroundTransparency = 1
menuTitle.Text = "★ NEON HUB ★"
menuTitle.TextColor3 = Color3.fromRGB(200, 200, 220)
menuTitle.Font = Enum.Font.SourceSansBold
menuTitle.TextSize = 13
menuTitle.TextXAlignment = Enum.TextXAlignment.Left

-- 👁️ МАЛЕНЬКА КРУГЛА КНОПКА (ПЕРЕТЯГУЄТЬСЯ)
local openBtn = Instance.new("TextButton")
openBtn.Name = "OpenButton"
openBtn.Size = UDim2.new(0, 42, 0, 42)
openBtn.Position = UDim2.new(0, 20, 0, 20)
openBtn.BackgroundColor3 = Color3.fromRGB(0, 140, 255)
openBtn.Text = "⚡"
openBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
openBtn.Font = Enum.Font.SourceSansBold
openBtn.TextSize = 18
openBtn.BorderSizePixel = 0
openBtn.Visible = false
openBtn.Active = true
openBtn.ZIndex = 10
openBtn.Parent = mainGui

Instance.new("UICorner", openBtn).CornerRadius = UDim.new(1, 0)
makeElementDraggable(openBtn)

-- ✕ КНОПКА ПОВНОГО ЗАКРИТТЯ
local closeBtn = Instance.new("TextButton", pF)
closeBtn.Name = "CloseBtn"
closeBtn.Size = UDim2.new(0, 20, 0, 20)
closeBtn.Position = UDim2.new(1, -28, 0, 6)
closeBtn.BackgroundColor3 = Color3.fromRGB(230, 45, 70)
closeBtn.Text = "✕"
closeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
closeBtn.Font = Enum.Font.SourceSansBold
closeBtn.TextSize = 11
closeBtn.BorderSizePixel = 0
Instance.new("UICorner", closeBtn).CornerRadius = UDim.new(0, 4)

closeBtn.MouseButton1Click:Connect(function()
    mainGui:Destroy()
end)

-- ─ КНОПКА ЗГОРТАННЯ
local minimizeBtn = Instance.new("TextButton", pF)
minimizeBtn.Name = "MinimizeBtn"
minimizeBtn.Size = UDim2.new(0, 20, 0, 20)
minimizeBtn.Position = UDim2.new(1, -54, 0, 6)
minimizeBtn.BackgroundColor3 = Color3.fromRGB(240, 180, 40)
minimizeBtn.Text = "─"
minimizeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
minimizeBtn.Font = Enum.Font.SourceSansBold
minimizeBtn.TextSize = 11
minimizeBtn.BorderSizePixel = 0
Instance.new("UICorner", minimizeBtn).CornerRadius = UDim.new(0, 4)

minimizeBtn.MouseButton1Click:Connect(function()
    pF.Visible = false
    openBtn.Visible = true
end)

openBtn.MouseButton1Click:Connect(function()
    openBtn.Visible = false
    pF.Visible = true
end)

-- ЗОНА ПРОКРУТКИ ДЛЯ ФУНКЦІЙ
local scrollFrame = Instance.new("ScrollingFrame", pF)
scrollFrame.Name = "Container"
scrollFrame.Size = UDim2.new(1, 0, 1, -40)
scrollFrame.Position = UDim2.new(0, 0, 0, 35)
scrollFrame.BackgroundTransparency = 1
scrollFrame.BorderSizePixel = 0
scrollFrame.CanvasSize = UDim2.new(0, 0, 0, 430) 
scrollFrame.ScrollBarThickness = 4
scrollFrame.ScrollBarImageColor3 = Color3.fromRGB(0, 170, 255)

local layout = Instance.new("UIListLayout", scrollFrame)
layout.SortOrder = Enum.SortOrder.LayoutOrder
layout.Padding = UDim.new(0, 10)

local padding = Instance.new("UIPadding", scrollFrame)
padding.PaddingTop = UDim.new(0, 5)
padding.PaddingBottom = UDim.new(0, 15)
padding.PaddingLeft = UDim.new(0, 12)
padding.PaddingRight = UDim.new(0, 12)

-- Змінні стану читів
local jp = false
local fl = false
local sp = false
local noclip = false
local autoEggs = false 
local curJmp = 50
local curFly = 311
local curSpd = 16
local flC

local function getC()
    local char = lp.Character
    if char then
        local hum = char:FindFirstChildOfClass("Humanoid")
        local root = char:FindFirstChild("HumanoidRootPart")
        return char, hum, root
    end
    return nil, nil, nil
end

-- НАДІЙНА ФУНКЦІЯ СЛАЙДЕРА З АВТОПЕРЕМІЩЕННЯМ
local function setupSlider(bg, btn, tl, min, max, default, prefix, callback)
    local dragging = false
    
    local function updateVisuals(value)
        local clampedValue = math.clamp(value, min, max)
        local percent = (clampedValue - min) / (max - min)
        btn.Position = UDim2.new(percent, -4, 0.5, -6)
        tl.Text = prefix .. tostring(clampedValue)
        callback(clampedValue)
    end
    
    local function updateFromMouse(input)
        local pos = math.clamp((input.Position.X - bg.AbsolutePosition.X) / bg.AbsoluteSize.X, 0, 1)
        local value = math.floor(min + (max - min) * pos)
        updateVisuals(value)
    end
    
    btn.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then dragging = true end
    end)
    U.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then dragging = false end
    end)
    U.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then updateFromMouse(input) end
    end)
    
    local function onTextUpdate()
        local rawText = tl.Text:gsub(prefix, "")
        local numValue = tonumber(rawText)
        if numValue then
            updateVisuals(numValue)
        end
    end

    tl.FocusLost:Connect(onTextUpdate)
    tl:GetPropertyChangedSignal("Text"):Connect(function()
        if not tl:IsFocused() then onTextUpdate() end
    end)
    
    updateVisuals(default)
end

-- ====================================================
-- СТВОРЕННЯ ФУНКЦІЙ ЧИТУ
-- ====================================================

-- СЛАЙДЕР ШВИДКОСТІ (SPEED)
local sFr = Instance.new("Frame", scrollFrame)
sFr.LayoutOrder = 1
sFr.Size = UDim2.new(1, 0, 0, 50)
sFr.BackgroundColor3 = Color3.fromRGB(30, 30, 38)
sFr.BackgroundTransparency = 0.4
sFr.BorderSizePixel = 0
Instance.new("UICorner", sFr).CornerRadius = UDim.new(0, 6)

local sActBtn = Instance.new("TextButton", sFr)
sActBtn.Size = UDim2.new(1, 0, 0, 18)
sActBtn.BackgroundColor3 = Color3.fromRGB(45, 45, 55)
sActBtn.Text = "🔴 SPEED"
sActBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
sActBtn.Font = Enum.Font.SourceSansBold
sActBtn.TextSize = 11
sActBtn.BorderSizePixel = 0
Instance.new("UICorner", sActBtn).CornerRadius = UDim.new(0, 4)

local sTl = Instance.new("TextBox", sFr)
sTl.Size = UDim2.new(1, 0, 0, 14)
sTl.Position = UDim2.new(0, 0, 0, 18)
sTl.BackgroundTransparency = 1
sTl.Text = "Speed: 16"
sTl.TextColor3 = Color3.fromRGB(255, 170, 0)
sTl.Font = Enum.Font.SourceSansBold
sTl.TextSize = 12
sTl.ClearTextOnFocus = false

local sBg = Instance.new("Frame", sFr)
sBg.Size = UDim2.new(0.85, 0, 0, 2)
sBg.Position = UDim2.new(0.075, 0, 0, 38)
sBg.BackgroundColor3 = Color3.fromRGB(255, 170, 0)
sBg.BorderSizePixel = 0

local sBtn = Instance.new("TextButton", sBg)
sBtn.Size = UDim2.new(0, 8, 0, 12)
sBtn.Position = UDim2.new(0.01, -4, 0.5, -6)
sBtn.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
sBtn.Text = ""
sBtn.BorderSizePixel = 0
Instance.new("UICorner", sBtn).CornerRadius = UDim.new(0, 2)

sActBtn.MouseButton1Click:Connect(function()
    sp = not sp
    sActBtn.Text = sp and "🟢 SPEED" or "🔴 SPEED"
    sActBtn.BackgroundColor3 = sp and Color3.fromRGB(0, 170, 90) or Color3.fromRGB(45, 45, 55)
    local _, h = getC()
    if h then h.WalkSpeed = sp and curSpd or 16 end
end)

setupSlider(sBg, sBtn, sTl, 16, 2000000, 16, "Speed: ", function(v)
    curSpd = v
    local _, h = getC()
    if h and sp then h.WalkSpeed = v end
end)


-- СЛАЙДЕР СТРИБКІВ (JUMP)
local jFr=Instance.new("Frame",scrollFrame)
