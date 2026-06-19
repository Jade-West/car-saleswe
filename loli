local Players = game:GetService("Players")
local Workspace = game:GetService("Workspace")
local UserInputService = game:GetService("UserInputService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

-- ── Early utility declarations (used by both UI and logic) ──
local function formatNumber(n)
    return tostring(math.floor(n)):reverse():gsub("(%d%d%d)", "%1,"):reverse():gsub("^,", "")
end

local GARAGE_PLATFORMS = {
    { pos = Vector3.new(-284, 8, 4465),
      cf  = CFrame.new(-284.640533, 4.78788948, 4466.86475,
                        -0.999972105,0,-0.00750996405,0,1,0,0.00750996405,0,-0.999972105),
      label = "Platform A" },
    { pos = Vector3.new(-211, 8, 4465),
      cf  = CFrame.new(-211.262665, 4.78788948, 4466.31299,
                        -0.999972105,0,-0.00750996405,0,1,0,0.00750996405,0,-0.999972105),
      label = "Platform B" },
}

-- iWait removed (unused)
local TweenService = game:GetService("TweenService")
local player = Players.LocalPlayer
local pgui = player:WaitForChild("PlayerGui")

local Remote    = ReplicatedStorage:WaitForChild("Remotes"):WaitForChild("RequestUpgrade")
local FixRemote = ReplicatedStorage:WaitForChild("Remotes"):WaitForChild("RequestFix")
local PromptSellRemote = ReplicatedStorage:WaitForChild("Remotes"):WaitForChild("PromptSell")

local GARAGE_CFRAME = CFrame.new(-2634, 5, -863)
local BOOST_SPEED   = 350
local DEFAULT_SPEED = 16

-- ===========================
-- THEME  (compact dark professional)
-- ===========================
local BG_DARK    = Color3.fromRGB(10, 10, 14)
local BG_MID     = Color3.fromRGB(16, 16, 22)
local BG_PANEL   = Color3.fromRGB(20, 20, 28)
local ACCENT     = Color3.fromRGB(70, 140, 255)
local ACCENT_GRN = Color3.fromRGB(50, 200, 110)
local ACCENT_PRP = Color3.fromRGB(130, 80, 220)
local TEXT_WHITE = Color3.new(1, 1, 1)
local TEXT_GRAY  = Color3.fromRGB(120, 120, 145)

-- ===========================
-- SCREEN GUI & MAIN FRAME
-- ===========================
local screenGui = Instance.new("ScreenGui", pgui)
screenGui.Name = "HybridCarTool_V8"
screenGui.ResetOnSpawn = false
screenGui.DisplayOrder = 999999999

local glowShell = Instance.new("Frame", screenGui)
glowShell.Size = UDim2.new(0, 342, 0, 498)
glowShell.Position = UDim2.new(0.5, -171, 0.5, -231)
glowShell.BackgroundColor3 = Color3.fromRGB(40, 80, 180)
glowShell.BackgroundTransparency = 0.88
glowShell.BorderSizePixel = 0
Instance.new("UICorner", glowShell).CornerRadius = UDim.new(0, 14)

local mainFrame = Instance.new("Frame", screenGui)
mainFrame.Size = UDim2.new(0, 334, 0, 490)
mainFrame.Position = UDim2.new(0.5, -167, 0.5, -227)
mainFrame.BackgroundColor3 = BG_DARK
mainFrame.BorderSizePixel = 0
mainFrame.Active = true
mainFrame.ClipsDescendants = true
Instance.new("UICorner", mainFrame).CornerRadius = UDim.new(0, 10)

local frameStroke = Instance.new("UIStroke", mainFrame)
frameStroke.Color = Color3.fromRGB(45, 70, 130)
frameStroke.Thickness = 1
frameStroke.Transparency = 0

-- ===========================
-- HEADER  (compact, 38px)
-- ===========================
local header = Instance.new("Frame", mainFrame)
header.Size = UDim2.new(1, 0, 0, 38)
header.BackgroundColor3 = BG_MID
header.BorderSizePixel = 0
Instance.new("UICorner", header).CornerRadius = UDim.new(0, 10)

local accentLine = Instance.new("Frame", header)
accentLine.Size = UDim2.new(1, 0, 0, 2)
accentLine.Position = UDim2.new(0, 0, 1, -2)
accentLine.BackgroundColor3 = Color3.fromRGB(50, 100, 200)
accentLine.BorderSizePixel = 0

local title = Instance.new("TextLabel", header)
title.Size = UDim2.new(1, -110, 1, 0)
title.Position = UDim2.new(0, 10, 0, 0)
title.Text = "⚡ HYBRID TOOL"
title.TextColor3 = Color3.fromRGB(200, 210, 255)
title.Font = Enum.Font.GothamBold
title.TextSize = 13
title.BackgroundTransparency = 1
title.TextXAlignment = Enum.TextXAlignment.Left

local keyBadge = Instance.new("TextLabel", header)
keyBadge.Size = UDim2.new(0, 26, 0, 16)
keyBadge.Position = UDim2.new(0, 148, 0.5, -8)
keyBadge.Text = "[Q]"
keyBadge.TextColor3 = TEXT_GRAY
keyBadge.Font = Enum.Font.GothamBold
keyBadge.TextSize = 9
keyBadge.BackgroundColor3 = Color3.fromRGB(20, 20, 30)
keyBadge.BorderSizePixel = 0
Instance.new("UICorner", keyBadge).CornerRadius = UDim.new(0, 3)

local minimizeBtn = Instance.new("TextButton", header)
minimizeBtn.Size = UDim2.new(0, 26, 0, 24)
minimizeBtn.Position = UDim2.new(1, -60, 0.5, -12)
minimizeBtn.Text = "−"
minimizeBtn.TextColor3 = TEXT_GRAY
minimizeBtn.Font = Enum.Font.GothamBold
minimizeBtn.TextSize = 16
minimizeBtn.BackgroundColor3 = Color3.fromRGB(25, 25, 38)
minimizeBtn.BorderSizePixel = 0
Instance.new("UICorner", minimizeBtn).CornerRadius = UDim.new(0, 5)

local closeBtn = Instance.new("TextButton", header)
closeBtn.Size = UDim2.new(0, 26, 0, 24)
closeBtn.Position = UDim2.new(1, -30, 0.5, -12)
closeBtn.Text = "✕"
closeBtn.TextColor3 = Color3.fromRGB(220, 80, 80)
closeBtn.Font = Enum.Font.GothamBold
closeBtn.TextSize = 11
closeBtn.BackgroundColor3 = Color3.fromRGB(35, 15, 18)
closeBtn.BorderSizePixel = 0
Instance.new("UICorner", closeBtn).CornerRadius = UDim.new(0, 5)
Instance.new("UIStroke", closeBtn).Color = Color3.fromRGB(100, 30, 35)

-- ===========================
-- TAB BAR
-- ===========================
local tabBar = Instance.new("Frame", mainFrame)
tabBar.Size = UDim2.new(1, -16, 0, 30)
tabBar.Position = UDim2.new(0, 8, 0, 42)
tabBar.BackgroundColor3 = BG_MID
tabBar.BorderSizePixel = 0
Instance.new("UICorner", tabBar).CornerRadius = UDim.new(0, 7)
Instance.new("UIStroke", tabBar).Color = Color3.fromRGB(30, 30, 50)

local tabToolsBtn, tabTeleportsBtn, tabCalcBtn, tabFarmBtn
do
    local function makeTabBtn(label, xPos, xWidth)
        local btn = Instance.new("TextButton", tabBar)
        btn.Size = UDim2.new(xWidth, -3, 1, -6)
        btn.Position = UDim2.new(xPos, 2, 0, 3)
        btn.Text = label; btn.Font = Enum.Font.GothamBold; btn.TextSize = 10
        btn.BackgroundColor3 = BG_MID; btn.TextColor3 = TEXT_GRAY; btn.BorderSizePixel = 0
        Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 5)
        return btn
    end
    tabToolsBtn     = makeTabBtn("⚙ TOOLS",  0,     0.25)
    tabTeleportsBtn = makeTabBtn("📍 TP",    0.25,  0.25)
    tabCalcBtn      = makeTabBtn("🧮 CALC",  0.5,   0.25)
    tabFarmBtn      = makeTabBtn("🌾 FARM",  0.75,  0.25)
end

-- ===========================
-- CONTENT CONTAINER
-- ===========================
local contentFrame = Instance.new("Frame", mainFrame)
contentFrame.Size = UDim2.new(1, 0, 1, -76)
contentFrame.Position = UDim2.new(0, 0, 0, 76)
contentFrame.BackgroundTransparency = 1

-- ===========================
-- TAB 1: TOOLS
-- ===========================
local toolsTab = Instance.new("Frame", contentFrame)
toolsTab.Size = UDim2.new(1, 0, 1, 0)
toolsTab.BackgroundTransparency = 1
toolsTab.Visible = true

local scanSpeed = 2.5

-- Row: Scan Speed + Walkspeed toggles
local radarSpeedToggle = Instance.new("TextButton", toolsTab)
radarSpeedToggle.Size = UDim2.new(0.44, 0, 0, 34)
radarSpeedToggle.Position = UDim2.new(0.05, 0, 0, 8)
radarSpeedToggle.Text = "🕐 CAP: 2.5s"

local walkSpeedToggle = Instance.new("TextButton", toolsTab)
walkSpeedToggle.Size = UDim2.new(0.44, 0, 0, 34)
walkSpeedToggle.Position = UDim2.new(0.51, 0, 0, 8)
walkSpeedToggle.Text = "🏃 SPEED: OFF"
walkSpeedToggle.BackgroundColor3 = Color3.fromRGB(140, 0, 0)
walkSpeedToggle.TextColor3 = TEXT_WHITE
walkSpeedToggle.Font = Enum.Font.GothamBold
walkSpeedToggle.TextSize = 11
walkSpeedToggle.BorderSizePixel = 0
Instance.new("UICorner", walkSpeedToggle).CornerRadius = UDim.new(0, 8)

-- Scan button
local scanBtn = Instance.new("TextButton", toolsTab)
scanBtn.Size = UDim2.new(0.44, 0, 0, 38)
scanBtn.Position = UDim2.new(0.05, 0, 0, 50)
scanBtn.Text = "📡 SCAN"
scanBtn.BackgroundColor3 = ACCENT
scanBtn.TextColor3 = TEXT_WHITE
scanBtn.Font = Enum.Font.GothamBold
scanBtn.TextSize = 12
scanBtn.BorderSizePixel = 0
Instance.new("UICorner", scanBtn).CornerRadius = UDim.new(0, 8)
local scanBtnGrad = Instance.new("UIGradient", scanBtn)
scanBtnGrad.Color = ColorSequence.new({
    ColorSequenceKeypoint.new(0, Color3.fromRGB(0, 170, 255)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(0, 100, 200))
})
scanBtnGrad.Rotation = 90

local deepScanBtn = Instance.new("TextButton", toolsTab)
deepScanBtn.Size = UDim2.new(0.34, 0, 0, 38)
deepScanBtn.Position = UDim2.new(0.51, 0, 0, 50)
deepScanBtn.Text = "🔍 DEEP SCAN"
deepScanBtn.BackgroundColor3 = Color3.fromRGB(80, 40, 160)
deepScanBtn.TextColor3 = TEXT_WHITE
deepScanBtn.Font = Enum.Font.GothamBold
deepScanBtn.TextSize = 10
deepScanBtn.BorderSizePixel = 0
Instance.new("UICorner", deepScanBtn).CornerRadius = UDim.new(0, 8)

local deepScanStopBtn = Instance.new("TextButton", toolsTab)
deepScanStopBtn.Size = UDim2.new(0.08, 0, 0, 38)
deepScanStopBtn.Position = UDim2.new(0.87, 0, 0, 50)
deepScanStopBtn.Text = "■"
deepScanStopBtn.BackgroundColor3 = Color3.fromRGB(70, 15, 20)
deepScanStopBtn.TextColor3 = Color3.fromRGB(220, 80, 90)
deepScanStopBtn.Font = Enum.Font.GothamBold
deepScanStopBtn.TextSize = 14
deepScanStopBtn.BorderSizePixel = 0
Instance.new("UICorner", deepScanStopBtn).CornerRadius = UDim.new(0, 8)
Instance.new("UIStroke", deepScanStopBtn).Color = Color3.fromRGB(130, 35, 45)
local deepScanBtnGrad = Instance.new("UIGradient", deepScanBtn)
deepScanBtnGrad.Color = ColorSequence.new({
    ColorSequenceKeypoint.new(0, Color3.fromRGB(110, 55, 220)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(60, 20, 130))
})
deepScanBtnGrad.Rotation = 90
Instance.new("UIStroke", deepScanBtn).Color = Color3.fromRGB(140, 80, 255)

-- Upgrade button
local modBtn = Instance.new("TextButton", toolsTab)
modBtn.Size = UDim2.new(0.72, 0, 0, 38)
modBtn.Position = UDim2.new(0.05, 0, 0, 96)
modBtn.Text = "⚡ INSTANT UPGRADE CAR"
modBtn.BackgroundColor3 = ACCENT_GRN
modBtn.TextColor3 = TEXT_WHITE
modBtn.Font = Enum.Font.GothamBold
modBtn.TextSize = 12
modBtn.BorderSizePixel = 0

local modStopBtn = Instance.new("TextButton", toolsTab)
modStopBtn.Size = UDim2.new(0.16, 0, 0, 38)
modStopBtn.Position = UDim2.new(0.79, 0, 0, 96)
modStopBtn.Text = "■"
modStopBtn.BackgroundColor3 = Color3.fromRGB(70, 15, 20)
modStopBtn.TextColor3 = Color3.fromRGB(220, 80, 90)
modStopBtn.Font = Enum.Font.GothamBold
modStopBtn.TextSize = 16
modStopBtn.BorderSizePixel = 0
Instance.new("UICorner", modStopBtn).CornerRadius = UDim.new(0, 8)
Instance.new("UIStroke", modStopBtn).Color = Color3.fromRGB(130, 35, 45)
Instance.new("UICorner", modBtn).CornerRadius = UDim.new(0, 8)
local modBtnGrad = Instance.new("UIGradient", modBtn)
modBtnGrad.Color = ColorSequence.new({
    ColorSequenceKeypoint.new(0, Color3.fromRGB(0, 220, 140)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(0, 150, 80))
})
modBtnGrad.Rotation = 90

-- Cosmetics button hidden — functionality merged into INSTANT UPGRADE
local cosBtn = Instance.new("TextButton", toolsTab)
cosBtn.Size = UDim2.new(0, 0, 0, 0)
cosBtn.Visible = false
local cosBtnGrad = Instance.new("UIGradient", cosBtn)

-- ===========================
-- INSTANT UPGRADE TOGGLES (compact grid under the button)
-- All ON by default — user can turn off specific upgrades
-- Uses a shared upgrades table so farm and instant upgrade both respect it
-- ===========================
local MOD_PANEL_TOP = 142
local modTogglePanel = Instance.new("Frame", toolsTab)
modTogglePanel.Size = UDim2.new(0.9, 0, 0, 36)
modTogglePanel.Position = UDim2.new(0.05, 0, 0, MOD_PANEL_TOP)
modTogglePanel.BackgroundColor3 = Color3.fromRGB(14, 14, 22)
modTogglePanel.BorderSizePixel = 0
Instance.new("UICorner", modTogglePanel).CornerRadius = UDim.new(0, 6)
Instance.new("UIStroke", modTogglePanel).Color = Color3.fromRGB(35, 50, 35)

-- Shared upgrade state (also used by autofarm via AF_OPTS.upgrades which is set separately)
-- Here we maintain a local "modUpgrades" that controls the instant upgrade button
local modUpgrades = {
    Fix=true, Accel1=true, Accel2=true, Accel3=true,
    Engine1=true, Engine2=true, Nitro1=true, Nitro2=true, Nitro3=true,
    Paint=true, Light=true, NitroCol=true, Underglow=true,
}

local MOD_TOGGLE_DEFS = {
    {id="Fix",      lbl="🔧Fix"},
    {id="Accel1",   lbl="🏎A1"},
    {id="Accel2",   lbl="🏎A2"},
    {id="Accel3",   lbl="🏎A3"},
    {id="Engine1",  lbl="⚙E1"},
    {id="Engine2",  lbl="⚙E2"},
    {id="Nitro1",   lbl="💨N1"},
    {id="Nitro2",   lbl="💨N2"},
    {id="Nitro3",   lbl="💨N3"},
    {id="Paint",    lbl="🖌Pnt"},
    {id="Light",    lbl="💡Lgt"},
    {id="NitroCol", lbl="🔥NC"},
    {id="Underglow",lbl="🌈UG"},
}

-- Two rows of toggle chips
local perRow = 7
local chipRefreshers = {}
for idx, def in ipairs(MOD_TOGGLE_DEFS) do
    local col = (idx - 1) % perRow
    local row = math.floor((idx - 1) / perRow)
    local chip = Instance.new("TextButton", modTogglePanel)
    chip.Size = UDim2.new(1/perRow, -2, 0, 15)
    chip.Position = UDim2.new(col/perRow, 1, 0, 1 + row * 17)
    chip.Text = def.lbl
    chip.Font = Enum.Font.GothamBold
    chip.TextSize = 7
    chip.BorderSizePixel = 0
    Instance.new("UICorner", chip).CornerRadius = UDim.new(0, 3)
    local chipId = def.id
    local function doRefresh()
        if modUpgrades[chipId] then
            chip.BackgroundColor3 = Color3.fromRGB(0, 70, 35)
            chip.TextColor3 = Color3.fromRGB(80, 220, 120)
        else
            chip.BackgroundColor3 = Color3.fromRGB(40, 40, 55)
            chip.TextColor3 = Color3.fromRGB(100, 100, 130)
        end
    end
    chipRefreshers[chipId] = doRefresh
    doRefresh()
    chip.MouseButton1Click:Connect(function()
        modUpgrades[chipId] = not modUpgrades[chipId]
        doRefresh()
    end)
end

-- Resize panel to fit 2 rows
modTogglePanel.Size = UDim2.new(0.9, 0, 0, 2 + math.ceil(#MOD_TOGGLE_DEFS / perRow) * 17)

-- ===========================
-- SPLIT RESULT PANELS
-- ===========================
local MOD_PANEL_BOTTOM = MOD_PANEL_TOP + 2 + math.ceil(#MOD_TOGGLE_DEFS / perRow) * 17 + 6
local PANEL_TOP    = MOD_PANEL_BOTTOM
local PANEL_HEIGHT = 270
local leftScroll, rightScroll  -- pre-declared; assigned inside do block below
do
    local leftOuter = Instance.new("Frame", toolsTab)
    leftOuter.Size = UDim2.new(0.455, 0, 0, PANEL_HEIGHT)
    leftOuter.Position = UDim2.new(0.04, 0, 0, PANEL_TOP)
    leftOuter.BackgroundColor3 = BG_PANEL
    leftOuter.BorderSizePixel = 0
    Instance.new("UICorner", leftOuter).CornerRadius = UDim.new(0, 10)
    Instance.new("Frame", leftOuter).Size = UDim2.new(1, 0, 0, 3)
    local la = leftOuter:FindFirstChildOfClass("Frame")
    if la then la.BackgroundColor3 = Color3.fromRGB(0, 180, 255); la.BorderSizePixel = 0 end
    local leftTitle = Instance.new("TextLabel", leftOuter)
    leftTitle.Size = UDim2.new(1, 0, 0, 24)
    leftTitle.Position = UDim2.new(0, 0, 0, 5)
    leftTitle.Text = "💰 TOP 3 PRICE"
    leftTitle.TextColor3 = Color3.fromRGB(0, 200, 255)
    leftTitle.Font = Enum.Font.GothamBold
    leftTitle.TextSize = 11
    leftTitle.BackgroundTransparency = 1
    leftScroll = Instance.new("ScrollingFrame", leftOuter)
    leftScroll.Size = UDim2.new(1, -6, 1, -32)
    leftScroll.Position = UDim2.new(0, 3, 0, 30)
    leftScroll.BackgroundTransparency = 1
    leftScroll.ScrollBarThickness = 3
    leftScroll.CanvasSize = UDim2.new(0, 0, 0, 0)
    leftScroll.BorderSizePixel = 0
    local leftLayout = Instance.new("UIListLayout", leftScroll)
    leftLayout.Padding = UDim.new(0, 5)
    leftLayout.SortOrder = Enum.SortOrder.LayoutOrder
    leftLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
        leftScroll.CanvasSize = UDim2.new(0, 0, 0, leftLayout.AbsoluteContentSize.Y + 4)
    end)

    local rightOuter = Instance.new("Frame", toolsTab)
    rightOuter.Size = UDim2.new(0.455, 0, 0, PANEL_HEIGHT)
    rightOuter.Position = UDim2.new(0.505, 0, 0, PANEL_TOP)
    rightOuter.BackgroundColor3 = BG_PANEL
    rightOuter.BorderSizePixel = 0
    Instance.new("UICorner", rightOuter).CornerRadius = UDim.new(0, 10)
    local rightTitle = Instance.new("TextLabel", rightOuter)
    rightTitle.Size = UDim2.new(1, 0, 0, 24)
    rightTitle.Position = UDim2.new(0, 0, 0, 5)
    rightTitle.Text = "⭐ SPECIAL PLATES"
    rightTitle.TextColor3 = Color3.fromRGB(255, 200, 50)
    rightTitle.Font = Enum.Font.GothamBold
    rightTitle.TextSize = 11
    rightTitle.BackgroundTransparency = 1
    rightScroll = Instance.new("ScrollingFrame", rightOuter)
    rightScroll.Size = UDim2.new(1, -6, 1, -32)
    rightScroll.Position = UDim2.new(0, 3, 0, 30)
    rightScroll.BackgroundTransparency = 1
    rightScroll.ScrollBarThickness = 3
    rightScroll.CanvasSize = UDim2.new(0, 0, 0, 0)
    rightScroll.BorderSizePixel = 0
    local rightLayout = Instance.new("UIListLayout", rightScroll)
    rightLayout.Padding = UDim.new(0, 5)
    rightLayout.SortOrder = Enum.SortOrder.LayoutOrder
    rightLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
        rightScroll.CanvasSize = UDim2.new(0, 0, 0, rightLayout.AbsoluteContentSize.Y + 4)
    end)
end

-- Status bar
local statusBar = Instance.new("TextLabel", toolsTab)
statusBar.Size = UDim2.new(0.9, 0, 0, 22)
statusBar.Position = UDim2.new(0.05, 0, 1, -26)
statusBar.Text = "Ready"
statusBar.TextColor3 = TEXT_GRAY
statusBar.Font = Enum.Font.Gotham
statusBar.TextSize = 10
statusBar.BackgroundColor3 = Color3.fromRGB(18, 18, 26)
statusBar.BorderSizePixel = 0
Instance.new("UICorner", statusBar).CornerRadius = UDim.new(0, 6)

-- ===========================
-- TAB 2: TELEPORTS
-- ===========================
local teleportsTab, btnGarage, btnPlot, btnPlyDealer, btnPlyNeigh
do
    teleportsTab = Instance.new("ScrollingFrame", contentFrame)
    teleportsTab.Size = UDim2.new(1, 0, 1, 0)
    teleportsTab.BackgroundTransparency = 1
    teleportsTab.Visible = false
    teleportsTab.ScrollBarThickness = 3
    teleportsTab.CanvasSize = UDim2.new(0, 0, 0, 380)
    local tpListLayout = Instance.new("UIListLayout", teleportsTab)
    tpListLayout.Padding = UDim.new(0, 6)
    tpListLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
    tpListLayout.SortOrder = Enum.SortOrder.LayoutOrder
    local spacer = Instance.new("Frame", teleportsTab)
    spacer.Size = UDim2.new(1, 0, 0, 5)
    spacer.BackgroundTransparency = 1
    local function createTPLabel(text)
        local lbl = Instance.new("TextLabel", teleportsTab)
        lbl.Size = UDim2.new(0.9, 0, 0, 25)
        lbl.Text = text; lbl.Font = Enum.Font.GothamBold; lbl.TextSize = 12
        lbl.TextColor3 = ACCENT; lbl.BackgroundTransparency = 1
        lbl.TextXAlignment = Enum.TextXAlignment.Left
        return lbl
    end
    local function createTPButton(text)
        local btn = Instance.new("TextButton", teleportsTab)
        btn.Size = UDim2.new(0.9, 0, 0, 35); btn.Text = text
        btn.Font = Enum.Font.GothamMedium; btn.TextSize = 13
        btn.BackgroundColor3 = BG_PANEL; btn.TextColor3 = TEXT_WHITE
        btn.BorderSizePixel = 0
        Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 8)
        local stroke = Instance.new("UIStroke", btn)
        stroke.Color = Color3.fromRGB(40, 40, 60); stroke.Thickness = 1
        btn.MouseEnter:Connect(function() stroke.Color = ACCENT end)
        btn.MouseLeave:Connect(function() stroke.Color = Color3.fromRGB(40, 40, 60) end)
        return btn
    end
    createTPLabel("🚗 / 🧍  Car or Player")
    btnGarage = createTPButton("Garage")
    btnPlot   = createTPButton("Plot")
    local spacer2 = Instance.new("Frame", teleportsTab)
    spacer2.Size = UDim2.new(1, 0, 0, 6); spacer2.BackgroundTransparency = 1
    createTPLabel("🧍  Player Only")
    btnPlyDealer = createTPButton("To Dealership")
    btnPlyNeigh  = createTPButton("To Neighborhood")
end

-- ===========================
-- TAB 3: CALCULATOR (Auto Car Value)
-- ===========================
local calcTab = Instance.new("Frame", contentFrame)
calcTab.Size = UDim2.new(1, 0, 1, 0)
calcTab.BackgroundTransparency = 1
calcTab.Visible = false

-- Top bar row 1: level display + refresh button
local calcTopBar = Instance.new("Frame", calcTab)
calcTopBar.Size = UDim2.new(1, -20, 0, 36)
calcTopBar.Position = UDim2.new(0, 10, 0, 6)
calcTopBar.BackgroundColor3 = BG_PANEL
calcTopBar.BorderSizePixel = 0
Instance.new("UICorner", calcTopBar).CornerRadius = UDim.new(0, 8)

local levelDisplay = Instance.new("TextLabel", calcTopBar)
levelDisplay.Size = UDim2.new(0.6, 0, 1, 0)
levelDisplay.Position = UDim2.new(0, 10, 0, 0)
levelDisplay.Text = "Level: —  |  Margin: —"
levelDisplay.TextColor3 = Color3.fromRGB(0, 200, 255)
levelDisplay.Font = Enum.Font.GothamBold
levelDisplay.TextSize = 11
levelDisplay.BackgroundTransparency = 1
levelDisplay.TextXAlignment = Enum.TextXAlignment.Left

local refreshBtn = Instance.new("TextButton", calcTopBar)
refreshBtn.Size = UDim2.new(0.35, 0, 0, 26)
refreshBtn.Position = UDim2.new(0.63, 0, 0.5, -13)
refreshBtn.Text = "🔄 REFRESH"
refreshBtn.BackgroundColor3 = Color3.fromRGB(0, 75, 150)
refreshBtn.TextColor3 = TEXT_WHITE
refreshBtn.Font = Enum.Font.GothamBold
refreshBtn.TextSize = 11
refreshBtn.BorderSizePixel = 0
Instance.new("UICorner", refreshBtn).CornerRadius = UDim.new(0, 6)

-- Top bar row 2: hide-zero toggle
local calcFilterBar = Instance.new("Frame", calcTab)
calcFilterBar.Size = UDim2.new(1, -20, 0, 28)
calcFilterBar.Position = UDim2.new(0, 10, 0, 46)
calcFilterBar.BackgroundTransparency = 1
calcFilterBar.BorderSizePixel = 0

local hideZeroEnabled = false
local hideZeroBtn = Instance.new("TextButton", calcFilterBar)
hideZeroBtn.Size = UDim2.new(0.55, 0, 1, 0)
hideZeroBtn.Position = UDim2.new(0, 0, 0, 0)
hideZeroBtn.Text = "👁 SHOW ALL"
hideZeroBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 65)
hideZeroBtn.TextColor3 = TEXT_GRAY
hideZeroBtn.Font = Enum.Font.GothamBold
hideZeroBtn.TextSize = 10
hideZeroBtn.BorderSizePixel = 0
Instance.new("UICorner", hideZeroBtn).CornerRadius = UDim.new(0, 6)

local sortNote = Instance.new("TextLabel", calcFilterBar)
sortNote.Size = UDim2.new(0.42, 0, 1, 0)
sortNote.Position = UDim2.new(0.58, 0, 0, 0)
sortNote.Text = "↓ Highest first"
sortNote.TextColor3 = Color3.fromRGB(80, 80, 100)
sortNote.Font = Enum.Font.Gotham
sortNote.TextSize = 9
sortNote.BackgroundTransparency = 1
sortNote.TextXAlignment = Enum.TextXAlignment.Right

-- Car list scroll (shifted down for second filter bar)
local carScroll = Instance.new("ScrollingFrame", calcTab)
carScroll.Size = UDim2.new(1, -20, 1, -82)
carScroll.Position = UDim2.new(0, 10, 0, 80)
carScroll.BackgroundTransparency = 1
carScroll.ScrollBarThickness = 3
carScroll.CanvasSize = UDim2.new(0, 0, 0, 0)
carScroll.BorderSizePixel = 0
local carListLayout = Instance.new("UIListLayout", carScroll)
carListLayout.Padding = UDim.new(0, 6)
carListLayout.SortOrder = Enum.SortOrder.LayoutOrder
carListLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    carScroll.CanvasSize = UDim2.new(0, 0, 0, carListLayout.AbsoluteContentSize.Y + 6)
end)

local copyBtn = nil  -- declared here, created in buildCarList below

-- ===========================
-- TAB 4: AUTOFARM
-- ===========================
local farmTab = Instance.new("Frame", contentFrame)
farmTab.Size = UDim2.new(1, 0, 1, 0)
farmTab.BackgroundTransparency = 1
farmTab.Visible = false

-- Top: min price input row
local farmTopBar = Instance.new("Frame", farmTab)
farmTopBar.Size = UDim2.new(1, -16, 0, 30)
farmTopBar.Position = UDim2.new(0, 8, 0, 5)
farmTopBar.BackgroundColor3 = BG_MID
farmTopBar.BorderSizePixel = 0
Instance.new("UICorner", farmTopBar).CornerRadius = UDim.new(0, 7)
Instance.new("UIStroke", farmTopBar).Color = Color3.fromRGB(35, 50, 100)

local farmPriceLabel = Instance.new("TextLabel", farmTopBar)
farmPriceLabel.Size = UDim2.new(0.38, 0, 1, 0)
farmPriceLabel.Position = UDim2.new(0, 8, 0, 0)
farmPriceLabel.Text = "💲 MIN PRICE"
farmPriceLabel.TextColor3 = Color3.fromRGB(100, 140, 220)
farmPriceLabel.Font = Enum.Font.GothamBold
farmPriceLabel.TextSize = 10
farmPriceLabel.BackgroundTransparency = 1
farmPriceLabel.TextXAlignment = Enum.TextXAlignment.Left

local farmPriceInput = Instance.new("TextBox", farmTopBar)
farmPriceInput.Size = UDim2.new(0.57, 0, 0, 22)
farmPriceInput.Position = UDim2.new(0.40, 0, 0.5, -11)
farmPriceInput.PlaceholderText = "300k / 1m / 1.4m"
farmPriceInput.Text = "300000"
farmPriceInput.BackgroundColor3 = Color3.fromRGB(12, 18, 36)
farmPriceInput.TextColor3 = Color3.fromRGB(160, 190, 255)
farmPriceInput.PlaceholderColor3 = Color3.fromRGB(55, 70, 110)
farmPriceInput.Font = Enum.Font.GothamBold
farmPriceInput.TextSize = 11
farmPriceInput.BorderSizePixel = 0
Instance.new("UICorner", farmPriceInput).CornerRadius = UDim.new(0, 5)
Instance.new("UIStroke", farmPriceInput).Color = Color3.fromRGB(40, 65, 130)

-- Control row: Start / Stop / Autoloop
local farmCtrlBar = Instance.new("Frame", farmTab)
farmCtrlBar.Size = UDim2.new(1, -16, 0, 28)
farmCtrlBar.Position = UDim2.new(0, 8, 0, 39)
farmCtrlBar.BackgroundTransparency = 1
farmCtrlBar.BorderSizePixel = 0

local farmStartBtn = Instance.new("TextButton", farmCtrlBar)
farmStartBtn.Size = UDim2.new(0.48, 0, 1, 0)
farmStartBtn.Position = UDim2.new(0, 0, 0, 0)
farmStartBtn.Text = "▶ START"
farmStartBtn.BackgroundColor3 = Color3.fromRGB(20, 80, 45)
farmStartBtn.TextColor3 = Color3.fromRGB(80, 220, 130)
farmStartBtn.Font = Enum.Font.GothamBold
farmStartBtn.TextSize = 11
farmStartBtn.BorderSizePixel = 0
Instance.new("UICorner", farmStartBtn).CornerRadius = UDim.new(0, 6)
local farmStartGrad = Instance.new("UIGradient", farmStartBtn)
farmStartGrad.Color = ColorSequence.new({
    ColorSequenceKeypoint.new(0, Color3.fromRGB(22, 90, 50)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(12, 55, 28))
})
farmStartGrad.Rotation = 90
Instance.new("UIStroke", farmStartBtn).Color = Color3.fromRGB(40, 150, 80)

local farmStopBtn = Instance.new("TextButton", farmCtrlBar)
farmStopBtn.Size = UDim2.new(0.27, 0, 1, 0)
farmStopBtn.Position = UDim2.new(0.5, 0, 0, 0)
farmStopBtn.Text = "■ STOP"
farmStopBtn.BackgroundColor3 = Color3.fromRGB(70, 15, 20)
farmStopBtn.TextColor3 = Color3.fromRGB(220, 80, 90)
farmStopBtn.Font = Enum.Font.GothamBold
farmStopBtn.TextSize = 10
farmStopBtn.BorderSizePixel = 0
Instance.new("UICorner", farmStopBtn).CornerRadius = UDim.new(0, 6)
Instance.new("UIStroke", farmStopBtn).Color = Color3.fromRGB(130, 35, 45)

local farmLoopBtn = Instance.new("TextButton", farmCtrlBar)
farmLoopBtn.Size = UDim2.new(0.2, 0, 1, 0)
farmLoopBtn.Position = UDim2.new(0.79, 0, 0, 0)
farmLoopBtn.Text = "🔁"
farmLoopBtn.BackgroundColor3 = Color3.fromRGB(22, 22, 35)
farmLoopBtn.TextColor3 = TEXT_GRAY
farmLoopBtn.Font = Enum.Font.GothamBold
farmLoopBtn.TextSize = 12
farmLoopBtn.BorderSizePixel = 0
Instance.new("UICorner", farmLoopBtn).CornerRadius = UDim.new(0, 6)
Instance.new("UIStroke", farmLoopBtn).Color = Color3.fromRGB(40, 40, 65)

-- Resume row (shows only when there's a saved state)
local farmResumeBar = Instance.new("Frame", farmTab)
farmResumeBar.Size = UDim2.new(1, -16, 0, 24)
farmResumeBar.Position = UDim2.new(0, 8, 0, 71)
farmResumeBar.BackgroundTransparency = 1
farmResumeBar.Visible = false

local farmResumeBtn = Instance.new("TextButton", farmResumeBar)
farmResumeBtn.Size = UDim2.new(1, 0, 1, 0)
farmResumeBtn.Text = "↩ RESUME LAST CYCLE"
farmResumeBtn.BackgroundColor3 = Color3.fromRGB(140, 80, 0)
farmResumeBtn.TextColor3 = TEXT_WHITE
farmResumeBtn.Font = Enum.Font.GothamBold
farmResumeBtn.TextSize = 11
farmResumeBtn.BorderSizePixel = 0
Instance.new("UICorner", farmResumeBtn).CornerRadius = UDim.new(0, 8)

-- "Use My Car" button — skips scan/buy, processes current car
local farmMyCarBar = Instance.new("Frame", farmTab)
farmMyCarBar.Size = UDim2.new(1, -16, 0, 24)
farmMyCarBar.Position = UDim2.new(0, 8, 0, 99)
farmMyCarBar.BackgroundTransparency = 1

local farmMyCarBtn = Instance.new("TextButton", farmMyCarBar)
farmMyCarBtn.Size = UDim2.new(1, 0, 1, 0)
farmMyCarBtn.Text = "🚗 PROCESS MY CURRENT CAR"
farmMyCarBtn.BackgroundColor3 = Color3.fromRGB(0, 80, 160)
farmMyCarBtn.TextColor3 = TEXT_WHITE
farmMyCarBtn.Font = Enum.Font.GothamBold
farmMyCarBtn.TextSize = 11
farmMyCarBtn.BorderSizePixel = 0
Instance.new("UICorner", farmMyCarBtn).CornerRadius = UDim.new(0, 8)
local farmMyCarGrad = Instance.new("UIGradient", farmMyCarBtn)
farmMyCarGrad.Color = ColorSequence.new({
    ColorSequenceKeypoint.new(0, Color3.fromRGB(30, 120, 220)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(0,  60, 140))
})
farmMyCarGrad.Rotation = 90

-- Options toggle row
local farmOptRow = Instance.new("Frame", farmTab)
farmOptRow.Size = UDim2.new(1, -16, 0, 20)
farmOptRow.Position = UDim2.new(0, 8, 0, 127)
farmOptRow.BackgroundTransparency = 1

local farmOptToggleBtn = Instance.new("TextButton", farmOptRow)
farmOptToggleBtn.Size = UDim2.new(0.48, 0, 1, 0)
farmOptToggleBtn.Position = UDim2.new(0, 0, 0, 0)
farmOptToggleBtn.Text = "⚙ OPTIONS ▼"
farmOptToggleBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 70)
farmOptToggleBtn.TextColor3 = TEXT_GRAY
farmOptToggleBtn.Font = Enum.Font.GothamBold
farmOptToggleBtn.TextSize = 10
farmOptToggleBtn.BorderSizePixel = 0
Instance.new("UICorner", farmOptToggleBtn).CornerRadius = UDim.new(0, 6)

-- Step indicator
local farmStepLabel = Instance.new("TextLabel", farmTab)
farmStepLabel.Size = UDim2.new(0.5, -4, 1, 0)
farmStepLabel.Position = UDim2.new(0.52, 0, 0, 0)
farmStepLabel.Parent = farmOptRow
farmStepLabel.Text = "⏸ Idle"
farmStepLabel.TextColor3 = ACCENT
farmStepLabel.Font = Enum.Font.GothamBold
farmStepLabel.TextSize = 10
farmStepLabel.BackgroundTransparency = 1
farmStepLabel.TextXAlignment = Enum.TextXAlignment.Right

-- Options panel (collapsible, sits on top of log area)
local farmOptPanel = Instance.new("ScrollingFrame", farmTab)
farmOptPanel.Size = UDim2.new(1, -16, 1, -300)
farmOptPanel.Position = UDim2.new(0, 8, 0, 151)
farmOptPanel.BackgroundColor3 = Color3.fromRGB(14, 14, 20)
farmOptPanel.BorderSizePixel = 0
farmOptPanel.ScrollBarThickness = 3
farmOptPanel.CanvasSize = UDim2.new(0, 0, 0, 0)
farmOptPanel.Visible = false
Instance.new("UICorner", farmOptPanel).CornerRadius = UDim.new(0, 8)
local farmOptLayout = Instance.new("UIListLayout", farmOptPanel)
farmOptLayout.SortOrder = Enum.SortOrder.LayoutOrder
farmOptLayout.Padding = UDim.new(0, 3)
farmOptLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    farmOptPanel.CanvasSize = UDim2.new(0, 0, 0, farmOptLayout.AbsoluteContentSize.Y + 6)
end)
Instance.new("UIPadding", farmOptPanel).PaddingTop = UDim.new(0, 4)

-- Helper: section header
local function optSection(title)
    local lbl = Instance.new("TextLabel", farmOptPanel)
    lbl.Size = UDim2.new(1, -8, 0, 18)
    lbl.Text = title
    lbl.TextColor3 = Color3.fromRGB(180,180,180)
    lbl.Font = Enum.Font.GothamBold
    lbl.TextSize = 10
    lbl.BackgroundTransparency = 1
    lbl.TextXAlignment = Enum.TextXAlignment.Left
    return lbl
end

-- Helper: toggle button; returns button and getter
local function optToggle(labelOn, labelOff, default, parent)
    parent = parent or farmOptPanel
    local row = Instance.new("Frame", parent)
    row.Size = UDim2.new(1, -8, 0, 22)
    row.BackgroundTransparency = 1
    local state = default
    local btn = Instance.new("TextButton", row)
    btn.Size = UDim2.new(1, 0, 1, 0)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 10
    btn.BorderSizePixel = 0
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 5)
    local function refresh()
        if state then
            btn.Text = "✅ " .. labelOn
            btn.BackgroundColor3 = Color3.fromRGB(0, 100, 50)
            btn.TextColor3 = TEXT_WHITE
        else
            btn.Text = "☐ " .. labelOff
            btn.BackgroundColor3 = Color3.fromRGB(55, 55, 70)
            btn.TextColor3 = TEXT_GRAY
        end
    end
    refresh()
    btn.MouseButton1Click:Connect(function() state = not state; refresh() end)
    return btn, function() return state end
end

-- ── FEATURES ──
optSection("── FEATURES ──")
local _, getDoUpgrade   = optToggle("UPGRADE CAR (on)",   "UPGRADE CAR (off)",   true)
local _, getDoSell      = optToggle("SELL CAR (on)",      "SELL CAR (off)",      true)
local _, getDoAccept    = optToggle("AUTO-ACCEPT BUYERS (on — margin -3%)", "AUTO-ACCEPT BUYERS (off)", false)

-- ── UPGRADES ──
optSection("── UPGRADES (when UPGRADE on) ──")

-- ALL MAX toggle
local allMaxState = true
local allMaxBtn = Instance.new("TextButton", farmOptPanel)
allMaxBtn.Size = UDim2.new(1, -8, 0, 22)
allMaxBtn.Text = "🔲 TOGGLE ALL MAX"
allMaxBtn.BackgroundColor3 = Color3.fromRGB(80, 50, 0)
allMaxBtn.TextColor3 = TEXT_WHITE
allMaxBtn.Font = Enum.Font.GothamBold
allMaxBtn.TextSize = 10
allMaxBtn.BorderSizePixel = 0
Instance.new("UICorner", allMaxBtn).CornerRadius = UDim.new(0, 5)

-- Individual upgrade toggles
local upgToggles = {}
local upgDefs = {
    {id="Fix",       label="🔧 Fix Car"},
    {id="Accel1",    label="🏎 AccelLvl 1"},
    {id="Accel2",    label="🏎 AccelLvl 2"},
    {id="Accel3",    label="🏎 AccelLvl 3"},
    {id="Engine1",   label="⚙ EngineLvl 1"},
    {id="Engine2",   label="⚙ EngineLvl 2"},
    {id="Nitro1",    label="💨 NitroLvl 1"},
    {id="Nitro2",    label="💨 NitroLvl 2"},
    {id="Nitro3",    label="💨 NitroLvl 3"},
    {id="Paint",     label="🖌 Paint (Black)"},
    {id="Light",     label="💡 LightColor (Black)"},
    {id="NitroCol",  label="🔥 NitroColor (JetFlame)"},
    {id="Underglow", label="🌈 Underglow"},
}
local upgGetters = {}
for _, def in ipairs(upgDefs) do
    local btn, getter = optToggle(def.label .. " ON", def.label .. " OFF", true)
    upgToggles[def.id] = btn
    upgGetters[def.id] = getter
end

allMaxBtn.MouseButton1Click:Connect(function()
    allMaxState = not allMaxState
    for id, btn in pairs(upgToggles) do
        -- simulate a click to sync state
        local cur = upgGetters[id]()
        if cur ~= allMaxState then btn:GetPropertyChangedSignal("Text"):Wait() end
        -- Force state by firing the button
        if upgGetters[id]() ~= allMaxState then
            local conn; conn = btn.MouseButton1Click:Connect(function() conn:Disconnect() end)
            btn:GetPropertyChangedSignal("BackgroundColor3"):Wait()
        end
    end
    -- Direct approach: just fire each one if state doesn't match
    for id, btn in pairs(upgToggles) do
        if upgGetters[id]() ~= allMaxState then
            btn:GetPropertyChangedSignal("BackgroundColor3"):Wait()
        end
    end
end)

-- Simpler allMax: track state directly
local upgStates = {}
for _, def in ipairs(upgDefs) do upgStates[def.id] = true end

-- Replace optToggle above with direct state tracking
-- (We'll use upgStates table directly, buttons just flip it)
-- Rebuild properly:
farmOptPanel:ClearAllChildren()
Instance.new("UIListLayout", farmOptPanel).SortOrder = Enum.SortOrder.LayoutOrder
Instance.new("UIPadding", farmOptPanel).PaddingTop = UDim.new(0, 4)
farmOptLayout = farmOptPanel:FindFirstChildOfClass("UIListLayout")
farmOptLayout.Padding = UDim.new(0, 3)
farmOptLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    farmOptPanel.CanvasSize = UDim2.new(0, 0, 0, farmOptLayout.AbsoluteContentSize.Y + 6)
end)

-- Shared opts table (all logic reads from this)
local AF_OPTS = {
    doUpgrade  = true,
    doSell     = true,
    doAccept   = false,
    upgrades   = {
        Fix=true, Accel1=true, Accel2=true, Accel3=true,
        Engine1=true, Engine2=true, Nitro1=true, Nitro2=true, Nitro3=true,
        Paint=true, Light=true, NitroCol=true, Underglow=true,
    }
}

do
local function makeOptToggle(parent, labelOn, labelOff, initState, onChanged)
    local row = Instance.new("Frame", parent)
    row.Size = UDim2.new(1, -8, 0, 22)
    row.BackgroundTransparency = 1
    local st = initState
    local b = Instance.new("TextButton", row)
    b.Size = UDim2.new(1, 0, 1, 0)
    b.Font = Enum.Font.GothamBold
    b.TextSize = 10
    b.BorderSizePixel = 0
    Instance.new("UICorner", b).CornerRadius = UDim.new(0, 5)
    local function upd()
        if st then
            b.Text = "✅ " .. labelOn
            b.BackgroundColor3 = Color3.fromRGB(0, 100, 50)
            b.TextColor3 = TEXT_WHITE
        else
            b.Text = "☐ " .. labelOff
            b.BackgroundColor3 = Color3.fromRGB(55, 55, 70)
            b.TextColor3 = TEXT_GRAY
        end
    end
    upd()
    b.MouseButton1Click:Connect(function()
        st = not st
        upd()
        if onChanged then onChanged(st) end
    end)
    return b, function() return st end, function(v) st=v; upd() end
end

-- Section header helper
local function makeSection(text)
    local l = Instance.new("TextLabel", farmOptPanel)
    l.Size = UDim2.new(1, -8, 0, 16)
    l.Text = text
    l.TextColor3 = Color3.fromRGB(160, 160, 200)
    l.Font = Enum.Font.GothamBold
    l.TextSize = 9
    l.BackgroundTransparency = 1
    l.TextXAlignment = Enum.TextXAlignment.Left
end

makeSection("── FEATURES ──")
makeOptToggle(farmOptPanel, "UPGRADE CAR ON", "UPGRADE CAR OFF", true, function(v) AF_OPTS.doUpgrade=v end)
makeOptToggle(farmOptPanel, "SELL CAR ON", "SELL CAR OFF", true, function(v) AF_OPTS.doSell=v end)
makeOptToggle(farmOptPanel, "AUTO-ACCEPT BUYERS ON (margin −3%)", "AUTO-ACCEPT BUYERS OFF", false, function(v) AF_OPTS.doAccept=v end)

makeSection("── UPGRADES ──")
-- ALL MAX / ALL OFF button
local allMaxBtnFinal = Instance.new("TextButton", farmOptPanel)
allMaxBtnFinal.Size = UDim2.new(1, -8, 0, 22)
allMaxBtnFinal.Text = "✅ ALL UPGRADES ON"
allMaxBtnFinal.BackgroundColor3 = Color3.fromRGB(0, 80, 40)
allMaxBtnFinal.TextColor3 = TEXT_WHITE
allMaxBtnFinal.Font = Enum.Font.GothamBold
allMaxBtnFinal.TextSize = 10
allMaxBtnFinal.BorderSizePixel = 0
Instance.new("UICorner", allMaxBtnFinal).CornerRadius = UDim.new(0, 5)

local upgSetters = {}
local upgDefs2 = {
    {id="Fix",       lbl="🔧 Fix Car"},
    {id="Accel1",    lbl="🏎 AccelLvl 1"},
    {id="Accel2",    lbl="🏎 AccelLvl 2"},
    {id="Accel3",    lbl="🏎 AccelLvl 3"},
    {id="Engine1",   lbl="⚙ EngineLvl 1"},
    {id="Engine2",   lbl="⚙ EngineLvl 2"},
    {id="Nitro1",    lbl="💨 NitroLvl 1"},
    {id="Nitro2",    lbl="💨 NitroLvl 2"},
    {id="Nitro3",    lbl="💨 NitroLvl 3"},
    {id="Paint",     lbl="🖌 Paint (Black)"},
    {id="Light",     lbl="💡 LightColor"},
    {id="NitroCol",  lbl="🔥 NitroColor"},
    {id="Underglow", lbl="🌈 Underglow"},
}
for _, d in ipairs(upgDefs2) do
    local _, _, setter = makeOptToggle(farmOptPanel, d.lbl .. " ON", d.lbl .. " OFF", true, function(v)
        AF_OPTS.upgrades[d.id] = v
    end)
    upgSetters[d.id] = setter
end

local allMaxOn = true
allMaxBtnFinal.MouseButton1Click:Connect(function()
    allMaxOn = not allMaxOn
    for id, setter in pairs(upgSetters) do
        AF_OPTS.upgrades[id] = allMaxOn
        setter(allMaxOn)
    end
    allMaxBtnFinal.Text = allMaxOn and "✅ ALL UPGRADES ON" or "☐ ALL UPGRADES OFF"
    allMaxBtnFinal.BackgroundColor3 = allMaxOn and Color3.fromRGB(0,80,40) or Color3.fromRGB(55,55,70)
end)

end
-- Options panel toggle (wired below, after farmLogScroll is declared)
local farmOptOpen = false

-- Log scroll
local farmLogScroll = Instance.new("ScrollingFrame", farmTab)
farmLogScroll.Size = UDim2.new(1, -16, 1, -300)
farmLogScroll.Position = UDim2.new(0, 8, 0, 151)
farmLogScroll.BackgroundColor3 = Color3.fromRGB(6, 6, 12)
farmLogScroll.BorderSizePixel = 0
farmLogScroll.ScrollBarThickness = 3
farmLogScroll.CanvasSize = UDim2.new(0, 0, 0, 0)
Instance.new("UICorner", farmLogScroll).CornerRadius = UDim.new(0, 8)
local farmLogLayout = Instance.new("UIListLayout", farmLogScroll)
farmLogLayout.SortOrder = Enum.SortOrder.LayoutOrder
farmLogLayout.Padding = UDim.new(0, 2)
farmLogLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    farmLogScroll.CanvasSize = UDim2.new(0, 0, 0, farmLogLayout.AbsoluteContentSize.Y + 4)
    farmLogScroll.CanvasPosition = Vector2.new(0, math.max(0, farmLogLayout.AbsoluteContentSize.Y - farmLogScroll.AbsoluteSize.Y))
end)

-- Wire options panel toggle now that both farmOptPanel and farmLogScroll exist
farmOptToggleBtn.MouseButton1Click:Connect(function()
    farmOptOpen = not farmOptOpen
    farmOptPanel.Visible = farmOptOpen
    farmLogScroll.Visible = not farmOptOpen
    farmOptToggleBtn.Text = farmOptOpen and "⚙ OPTIONS ▲" or "⚙ OPTIONS ▼"
    farmOptToggleBtn.BackgroundColor3 = farmOptOpen and Color3.fromRGB(80,50,140) or Color3.fromRGB(50,50,70)
end)

-- ── PROFIT TABLE (bottom of farm tab) ──
local profitPanel = Instance.new("Frame", farmTab)
profitPanel.Size = UDim2.new(1, -16, 0, 145)
profitPanel.Position = UDim2.new(0, 8, 1, -149)
profitPanel.BackgroundColor3 = Color3.fromRGB(6, 6, 12)
profitPanel.BorderSizePixel = 0
Instance.new("UICorner", profitPanel).CornerRadius = UDim.new(0, 8)

-- Header row
local profitHeader = Instance.new("Frame", profitPanel)
profitHeader.Size = UDim2.new(1, 0, 0, 22)
profitHeader.Position = UDim2.new(0, 0, 0, 0)
profitHeader.BackgroundColor3 = Color3.fromRGB(10, 10, 24)
profitHeader.BorderSizePixel = 0
Instance.new("UICorner", profitHeader).CornerRadius = UDim.new(0, 8)

local function makeHeaderCol(text, xScale, wScale, align)
    local l = Instance.new("TextLabel", profitHeader)
    l.Size = UDim2.new(wScale, 0, 1, 0)
    l.Position = UDim2.new(xScale, 4, 0, 0)
    l.Text = text
    l.TextColor3 = Color3.fromRGB(160, 160, 200)
    l.Font = Enum.Font.GothamBold
    l.TextSize = 9
    l.BackgroundTransparency = 1
    l.TextXAlignment = align or Enum.TextXAlignment.Left
end
makeHeaderCol("🏎 Car",   0,    0.38)
makeHeaderCol("Cost",     0.38, 0.2,  Enum.TextXAlignment.Right)
makeHeaderCol("Sold",     0.58, 0.2,  Enum.TextXAlignment.Right)
makeHeaderCol("Profit",   0.78, 0.22, Enum.TextXAlignment.Right)

-- Rows scroll area
local profitRowsScroll = Instance.new("ScrollingFrame", profitPanel)
profitRowsScroll.Size = UDim2.new(1, 0, 1, -44)
profitRowsScroll.Position = UDim2.new(0, 0, 0, 22)
profitRowsScroll.BackgroundTransparency = 1
profitRowsScroll.BorderSizePixel = 0
profitRowsScroll.ScrollBarThickness = 3
profitRowsScroll.CanvasSize = UDim2.new(0, 0, 0, 0)
local profitRowsLayout = Instance.new("UIListLayout", profitRowsScroll)
profitRowsLayout.SortOrder = Enum.SortOrder.LayoutOrder
profitRowsLayout.Padding = UDim.new(0, 1)
profitRowsLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    profitRowsScroll.CanvasSize = UDim2.new(0, 0, 0, profitRowsLayout.AbsoluteContentSize.Y + 2)
    profitRowsScroll.CanvasPosition = Vector2.new(0, math.max(0, profitRowsLayout.AbsoluteContentSize.Y - profitRowsScroll.AbsoluteSize.Y))
end)

-- Total bar
local profitTotalBar = Instance.new("Frame", profitPanel)
profitTotalBar.Size = UDim2.new(1, 0, 0, 22)
profitTotalBar.Position = UDim2.new(0, 0, 1, -22)
profitTotalBar.BackgroundColor3 = Color3.fromRGB(0, 60, 30)
profitTotalBar.BorderSizePixel = 0
Instance.new("UICorner", profitTotalBar).CornerRadius = UDim.new(0, 6)

local profitTotalLabel = Instance.new("TextLabel", profitTotalBar)
profitTotalLabel.Size = UDim2.new(0.5, 0, 1, 0)
profitTotalLabel.Position = UDim2.new(0, 6, 0, 0)
profitTotalLabel.Text = "SESSION PROFIT"
profitTotalLabel.TextColor3 = Color3.fromRGB(100, 200, 120)
profitTotalLabel.Font = Enum.Font.GothamBold
profitTotalLabel.TextSize = 10
profitTotalLabel.BackgroundTransparency = 1
profitTotalLabel.TextXAlignment = Enum.TextXAlignment.Left

local profitTotalValue = Instance.new("TextLabel", profitTotalBar)
profitTotalValue.Size = UDim2.new(0.5, -6, 1, 0)
profitTotalValue.Position = UDim2.new(0.5, 0, 0, 0)
profitTotalValue.Text = "$0"
profitTotalValue.TextColor3 = Color3.fromRGB(80, 255, 140)
profitTotalValue.Font = Enum.Font.GothamBold
profitTotalValue.TextSize = 10
profitTotalValue.BackgroundTransparency = 1
profitTotalValue.TextXAlignment = Enum.TextXAlignment.Right

-- Clear button
local profitClearBtn = Instance.new("TextButton", profitPanel)
profitClearBtn.Size = UDim2.new(1, 0, 0, 0)  -- hidden until there are rows
profitClearBtn.Position = UDim2.new(0, 0, 0, 0)
profitClearBtn.Text = ""
profitClearBtn.BackgroundTransparency = 1

-- Session data
local sessionProfit   = 0
local sessionRowCount = 0

local function addProfitRow(carName, cost, soldAt)
    cost   = tonumber(cost)   or 0
    soldAt = tonumber(soldAt) or 0
    sessionRowCount += 1
    local profit = soldAt - cost
    sessionProfit += profit

    local row = Instance.new("Frame", profitRowsScroll)
    row.Size = UDim2.new(1, 0, 0, 18)
    row.BackgroundColor3 = sessionRowCount % 2 == 0
        and Color3.fromRGB(16, 16, 26) or Color3.fromRGB(12, 12, 20)
    row.BorderSizePixel = 0
    row.LayoutOrder = sessionRowCount

    local function makeCell(text, xScale, wScale, col, align)
        local l = Instance.new("TextLabel", row)
        l.Size = UDim2.new(wScale, -4, 1, 0)
        l.Position = UDim2.new(xScale, 4, 0, 0)
        l.Text = text
        l.TextColor3 = col or TEXT_WHITE
        l.Font = Enum.Font.Gotham
        l.TextSize = 9
        l.BackgroundTransparency = 1
        l.TextXAlignment = align or Enum.TextXAlignment.Left
        l.TextTruncate = Enum.TextTruncate.AtEnd
    end

    -- Shorten car name
    local shortName = carName:match("{%x+%-[^}]+}(.+)") or carName
    shortName = shortName:gsub("^%s*", ""):sub(1, 22)

    local profitCol = profit >= 0 and Color3.fromRGB(80, 255, 140) or Color3.fromRGB(255, 80, 80)
    local profitSign = profit >= 0 and "+" or ""

    makeCell(shortName,                              0,    0.38)
    makeCell("$"..formatNumber(cost),               0.38, 0.2,  Color3.fromRGB(180,180,180), Enum.TextXAlignment.Right)
    makeCell("$"..formatNumber(soldAt),             0.58, 0.2,  Color3.fromRGB(220,220,100), Enum.TextXAlignment.Right)
    makeCell(profitSign.."$"..formatNumber(profit), 0.78, 0.22, profitCol,                   Enum.TextXAlignment.Right)

    -- Update total
    local totalSign = sessionProfit >= 0 and "+" or ""
    profitTotalValue.Text = totalSign .. "$" .. formatNumber(sessionProfit)
    profitTotalValue.TextColor3 = sessionProfit >= 0 and Color3.fromRGB(80,255,140) or Color3.fromRGB(255,80,80)
end

-- addProfitRow is called directly by farm logic below

-- ===========================
-- LOGIC: MINIMIZE
-- ===========================
local isMinimized = false
local FULL_H = 490
local MINI_H = 38

minimizeBtn.MouseButton1Click:Connect(function()
    isMinimized = not isMinimized
    local targetSize = isMinimized
        and UDim2.new(0, 334, 0, MINI_H)
        or  UDim2.new(0, 334, 0, FULL_H)
    minimizeBtn.Text = isMinimized and "+" or "−"
    TweenService:Create(mainFrame,
        TweenInfo.new(0.25, Enum.EasingStyle.Quart, Enum.EasingDirection.Out),
        { Size = targetSize }
    ):Play()
    tabBar.Visible        = not isMinimized
    contentFrame.Visible  = not isMinimized
    glowShell.Visible     = not isMinimized
end)

-- ===========================
-- LOGIC: TAB SWITCHING
-- ===========================
local function resetTabs()
    toolsTab.Visible     = false
    teleportsTab.Visible = false
    calcTab.Visible      = false
    farmTab.Visible      = false
    for _, b in ipairs({ tabToolsBtn, tabTeleportsBtn, tabCalcBtn, tabFarmBtn }) do
        b.BackgroundColor3 = BG_MID
        b.TextColor3 = TEXT_GRAY
        local s = b:FindFirstChildOfClass("UIStroke")
        if s then s:Destroy() end
    end
end
local function activateTab(btn, tab)
    resetTabs()
    tab.Visible = true
    btn.BackgroundColor3 = Color3.fromRGB(22, 32, 58)
    btn.TextColor3 = Color3.fromRGB(150, 180, 255)
    local s = Instance.new("UIStroke", btn)
    s.Color = Color3.fromRGB(55, 90, 185)
    s.Thickness = 1
end
tabToolsBtn.MouseButton1Click:Connect(function()     activateTab(tabToolsBtn,     toolsTab) end)
tabTeleportsBtn.MouseButton1Click:Connect(function() activateTab(tabTeleportsBtn, teleportsTab) end)
tabCalcBtn.MouseButton1Click:Connect(function()      activateTab(tabCalcBtn,      calcTab) end)
tabFarmBtn.MouseButton1Click:Connect(function()      activateTab(tabFarmBtn,      farmTab) end)
activateTab(tabToolsBtn, toolsTab)

-- ===========================
-- LOGIC: HELPERS
-- ===========================
-- (formatNumber declared at top of file)

local function isSpecialPlate(licText, licNum)
    -- LETTERS: all 3 must be the same (e.g. EEE, TTT)
    licText = tostring(licText):upper()
    local isSpecialText = false
    if #licText >= 3 then
        if licText:sub(1,1) == licText:sub(2,2) and licText:sub(2,2) == licText:sub(3,3) then
            isSpecialText = true
        end
    end

    -- NUMBERS:
    --   1 digit  → always special (e.g. 6)
    --   2 digits → always special (e.g. 31)
    --   3 digits → always special (e.g. 343)
    --   4 digits → special ONLY if all four are the same digit (e.g. 3333)
    --   5+ digits → not special
    licNum = tostring(licNum):gsub("%s+", "")  -- strip whitespace
    local len = #licNum
    local isSpecialNum = false
    if len >= 1 and len <= 3 then
        isSpecialNum = true
    elseif len == 4 then
        -- all four digits must be identical
        local first = licNum:sub(1,1)
        if licNum:sub(2,2) == first and licNum:sub(3,3) == first and licNum:sub(4,4) == first then
            isSpecialNum = true
        end
    end

    return isSpecialText or isSpecialNum
end

-- ===========================
-- LOGIC: SMART TELEPORT
-- Auto-detects: in car → vehicle teleport
--               on foot → player teleport
-- ===========================
local function getVehicle()
    local char = player.Character
    if not char then return nil end
    local hum = char:FindFirstChildOfClass("Humanoid")
    if not hum or not hum.SeatPart then return nil end
    local model = hum.SeatPart:FindFirstAncestorOfClass("Model")
    if model and not model.PrimaryPart then
        model.PrimaryPart = model:FindFirstChildWhichIsA("BasePart", true)
    end
    return model
end

local function teleportPlayer(cf)
    local char = player.Character
    if char and char:FindFirstChild("HumanoidRootPart") then
        char:PivotTo(cf)
    end
end

local function smartTeleport(cf)
    local veh = getVehicle()
    if veh then
        veh:PivotTo(cf)
    else
        teleportPlayer(cf)
    end
end

local function getPlotCFrame()
    local plotVal = player:FindFirstChild("Plot")
    if not plotVal then return nil end
    local plotName = (plotVal:IsA("StringValue") and plotVal.Value)
        or (plotVal:IsA("ObjectValue") and plotVal.Value and plotVal.Value.Name)
    local plots = Workspace:FindFirstChild("Plots")
    local plot  = plots and plots:FindFirstChild(plotName)
    local part  = plot and (plot.PrimaryPart or plot:FindFirstChildWhichIsA("BasePart"))
    if part then return part.CFrame + Vector3.new(0, 5, 0) end
end

local function getTeleportLocation(name)
    local folder = Workspace:FindFirstChild("TeleportLocations")
    if folder then
        local loc = folder:FindFirstChild(name)
        if loc then return loc.CFrame + Vector3.new(0, 5, 0) end
    end
end

-- Garage & Plot: smart (car or player)
btnGarage.MouseButton1Click:Connect(function()
    smartTeleport(GARAGE_CFRAME)
end)
btnPlot.MouseButton1Click:Connect(function()
    local cf = getPlotCFrame()
    if cf then smartTeleport(cf) end
end)

-- Dealership & Neighborhood: player only
btnPlyDealer.MouseButton1Click:Connect(function()
    local cf = getTeleportLocation("Dealership")
    if cf then smartTeleport(cf) end
end)
btnPlyNeigh.MouseButton1Click:Connect(function()
    local cf = getTeleportLocation("Neighborhood")
    if cf then smartTeleport(cf) end
end)

-- ===========================
-- LOGIC: SPEED TOGGLES
-- ===========================
radarSpeedToggle.MouseButton1Click:Connect(function()
    if scanSpeed == 2.5 then
        scanSpeed = 1.0
        radarSpeedToggle.Text = "⚡ CAP: 1.0s"
        radarSpeedToggle.BackgroundColor3 = Color3.fromRGB(180, 100, 0)
    else
        scanSpeed = 2.5
        radarSpeedToggle.Text = "🕐 CAP: 2.5s"
        radarSpeedToggle.BackgroundColor3 = Color3.fromRGB(50, 50, 65)
    end
end)

local walkSpeedEnabled = false
walkSpeedToggle.MouseButton1Click:Connect(function()
    walkSpeedEnabled = not walkSpeedEnabled
    local hum = player.Character and player.Character:FindFirstChildOfClass("Humanoid")
    if hum then
        if walkSpeedEnabled then
            hum.WalkSpeed = BOOST_SPEED
            walkSpeedToggle.Text = "🏃 SPEED: ON"
            walkSpeedToggle.BackgroundColor3 = Color3.fromRGB(0, 150, 0)
        else
            hum.WalkSpeed = DEFAULT_SPEED
            walkSpeedToggle.Text = "🏃 SPEED: OFF"
            walkSpeedToggle.BackgroundColor3 = Color3.fromRGB(140, 0, 0)
        end
    end
end)

-- ===========================
-- LOGIC: RADAR SCANNER
-- Left  → Top 3 highest price (scrollable)
-- Right → ALL special plates  (scrollable)
-- ===========================

-- ===========================
-- CAR DATABASE (in-game name → dealer min/max/rarity)
-- ===========================
local CAR_DB = {
    { make="Lamborghini", model="Revuelto", ingame="Rovalto", year="2023", rarity="🔒 Limited", pct="0.005%", minP=4400000, maxP=4500000 },
    { make="Pagani", model="Huayra", ingame="Heura", year="2014", rarity="🔒 Limited", pct="0.02%", minP=49700000, maxP=50000000 },
    { make="Rolls Royce", model="Ghost", ingame="Goster", year="2025", rarity="🔒 Limited", pct="0.03%", minP=5450000, maxP=5500000 },
    { make="Nissan", model="Ddsen", ingame="Dedsona", year="2016", rarity="🔒 Limited", pct="0.1%", minP=80000, maxP=80000 },
    { make="SSC", model="Tuatara", ingame="Teutara", year="2020", rarity="Standard", pct="0%", minP=7020000, maxP=7120000 },
    { make="Audi", model="RS7", ingame="Audira", year="2024", rarity="Standard", pct="0%", minP=730000, maxP=750000 },
    { make="Cadillac", model="Escalade", ingame="Escali", year="2024", rarity="Standard", pct="0%", minP=540000, maxP=560000 },
    { make="Infinity", model="QX80", ingame="Quxan", year="2022", rarity="Standard", pct="0%", minP=395000, maxP=425000 },
    { make="Audi", model="RS3", ingame="Audira", year="2023", rarity="Standard", pct="0%", minP=298000, maxP=312000 },
    { make="Dodge", model="Durango SRT Hellcat", ingame="Doringo", year="2021", rarity="Standard", pct="0%", minP=273000, maxP=286000 },
    { make="Toyota", model="Land Cruiser Prado", ingame="Pradr", year="2024", rarity="Standard", pct="0%", minP=251000, maxP=262000 },
    { make="Chevrolet", model="Tahoe", ingame="Tahiro", year="2024", rarity="Standard", pct="0%", minP=220000, maxP=240000 },
    { make="Chevrolet", model="Tahoe", ingame="Tahiro", year="2022", rarity="Standard", pct="0%", minP=222000, maxP=235000 },
    { make="Hyundai", model="Palisade", ingame="بلاسد", year="2026", rarity="Standard", pct="0%", minP=218000, maxP=230000 },
    { make="Cadillac", model="CT4", ingame="Cadira CT4", year="2024", rarity="Standard", pct="0%", minP=192000, maxP=216000 },
    { make="Range Rover", model="SV Autobiography", ingame="Rangera", year="2017", rarity="Standard", pct="0%", minP=172000, maxP=212000 },
    { make="Toyota", model="Crown", ingame="Kronza", year="2023", rarity="Standard", pct="0%", minP=163000, maxP=175000 },
    { make="Toyota", model="Land Cruiser J80", ingame="Genera J80", year="2022", rarity="Standard", pct="0%", minP=144000, maxP=155000 },
    { make="Nissan", model="Altima", ingame="Altera", year="2023", rarity="Standard", pct="0%", minP=133000, maxP=146000 },
    { make="Toyota", model="Camry", ingame="Kimora", year="2025", rarity="Standard", pct="0%", minP=128000, maxP=142000 },
    { make="Hyundai", model="Sonata", ingame="Sonera", year="2024", rarity="Standard", pct="0%", minP=129000, maxP=141000 },
    { make="Hyundai", model="Santa Fe", ingame="سنتاف", year="2025", rarity="Standard", pct="0%", minP=126000, maxP=135000 },
    { make="Genesis", model="GV70", ingame="Genera JV70", year="2022", rarity="Standard", pct="0%", minP=128000, maxP=135000 },
    { make="Kia", model="K4", ingame="C4", year="2025", rarity="Standard", pct="0%", minP=115000, maxP=124000 },
    { make="Toyota", model="Camry", ingame="Kimora", year="2025", rarity="Standard", pct="0%", minP=105340, maxP=115070 },
    { make="Hyundai", model="Elantra", ingame="Alantar", year="2024", rarity="Standard", pct="0%", minP=103000, maxP=115000 },
    { make="MG Motor", model="7", ingame="MGara 7", year="2025", rarity="Standard", pct="0%", minP=91050, maxP=113050 },
    { make="Kia", model="K5 GT-Line", ingame="Q5", year="2021", rarity="Standard", pct="0%", minP=95000, maxP=105000 },
    { make="Kia", model="Telluride", ingame="Telluri", year="2021", rarity="Standard", pct="0%", minP=92000, maxP=104000 },
    { make="Toyota", model="Camry", ingame="Kimora", year="2022", rarity="Standard", pct="0%", minP=88000, maxP=99000 },
    { make="Changan", model="UNI-V", ingame="Unix V", year="2024", rarity="Standard", pct="0%", minP=75000, maxP=95000 },
    { make="Kia", model="K5 GT-Line", ingame="Q5", year="2021", rarity="Standard", pct="0%", minP=75000, maxP=85200 },
    { make="Dodge", model="Charger SRT", ingame="Charjero CRT", year="2014", rarity="Standard", pct="0%", minP=75000, maxP=84000 },
    { make="Mazda", model="CX-5", ingame="Sportana", year="2023", rarity="Standard", pct="0%", minP=73000, maxP=82000 },
    { make="Hyundai", model="Sonata", ingame="Sonera", year="2020", rarity="Standard", pct="0%", minP=65000, maxP=74000 },
    { make="Genesis", model="G80", ingame="Genera J80", year="2015", rarity="Standard", pct="0%", minP=63000, maxP=72000 },
    { make="Mercedes-Benz", model="GL-Class", ingame="امبارا", year="2017", rarity="Standard", pct="0%", minP=58000, maxP=65000 },
    { make="Mazda", model="3", ingame="Mazdr", year="2024", rarity="Standard", pct="0%", minP=53800, maxP=62100 },
    { make="Toyota", model="Camry LE", ingame="Kimora", year="2018", rarity="Standard", pct="0%", minP=47000, maxP=55000 },
    { make="Chevrolet", model="Spark", ingame="Sbakir", year="2021", rarity="Standard", pct="0%", minP=31000, maxP=42000 },
    { make="Toyota", model="Camry", ingame="Kimora", year="2008", rarity="Standard", pct="0%", minP=25000, maxP=35000 },
    { make="Hyundai", model="Accent", ingame="ACCENJ", year="2012", rarity="Standard", pct="0%", minP=28000, maxP=32000 },
    { make="Ford", model="Crown Victoria", ingame="Crown Victora", year="2008", rarity="Standard", pct="0%", minP=14000, maxP=25500 },
    { make="Hyundai", model="Elantra", ingame="alantar", year="2011", rarity="Standard", pct="0%", minP=11000, maxP=21000 },
    { make="Mercedes-Benz", model="AMG ONE", ingame="Serao", year="2023", rarity="🌟 Legendary", pct="0.001%", minP=20500000, maxP=21000000 },
    { make="Bugatti", model="Chiron", ingame="Chiror", year="2020", rarity="🌟 Legendary", pct="0.003%", minP=10580000, maxP=10780000 },
    { make="Mercedes-Benz", model="Maybach Vision", ingame="Maybar", year="2016", rarity="🌟 Legendary", pct="0.005%", minP=11000000, maxP=12000000 },
    { make="McLaren", model="P1", ingame="Mekleren", year="2024", rarity="🌟 Legendary", pct="0.005%", minP=4440000, maxP=4520000 },
    { make="Bugatti", model="Divo", ingame="Divao", year="2022", rarity="💎 Ultra Rare", pct="0.01%", minP=21000000, maxP=22222222 },
    { make="Rolls-Royce", model="Specter", ingame="Spocra", year="2025", rarity="💎 Ultra Rare", pct="0.02%", minP=2950000, maxP=3040000 },
    { make="Porsche", model="GT3 RS", ingame="Borcha", year="2024", rarity="💎 Ultra Rare", pct="0.02%", minP=1295000, maxP=1330000 },
    { make="Lamborgini", model="Huracán", ingame="Lambo", year="2023", rarity="🔴 Very Rare", pct="0.03%", minP=1320000, maxP=1350000 },
    { make="Lamborghini", model="Aventador", ingame="Avandor", year="2024", rarity="🔴 Very Rare", pct="0.05%", minP=2101000, maxP=2250000 },
    { make="McLaren", model="765LT", ingame="Mekleren", year="2021", rarity="🔴 Very Rare", pct="0.05%", minP=1470000, maxP=1510000 },
    { make="Ferrari", model="F8", ingame="Varoni", year="2022", rarity="🔴 Very Rare", pct="0.05%", minP=1470000, maxP=1510000 },
    { make="Mercedes-Benz", model="S-Class", ingame="Baybar", year="2022", rarity="🟠 Rare", pct="0.07%", minP=2280000, maxP=2300000 },
    { make="Rolls-Royce", model="Cullinan", ingame="Culinar", year="2021", rarity="🟠 Rare", pct="0.08%", minP=1900000, maxP=1920000 },
    { make="Rolls-Royce", model="Phantom", ingame="Fantur", year="2020", rarity="🟠 Rare", pct="0.09%", minP=1910000, maxP=2050000 },
    { make="Ford", model="Mustang Dark Horse", ingame="Black Horse", year="2024", rarity="🟠 Rare", pct="0.1%", minP=310000, maxP=325000 },
    { make="Bentley", model="Bentayga", ingame="Bentyagr", year="2017", rarity="🟡 Semi-Rare", pct="0.2%", minP=305000, maxP=320000 },
    { make="Range Rover", model="Autobiography", ingame="Ranger", year="2023", rarity="🟡 Semi-Rare", pct="0.3%", minP=755000, maxP=780000 },
    { make="Bentley", model="Bentayga", ingame="Bentyag", year="2021", rarity="🟡 Semi-Rare", pct="0.4%", minP=890000, maxP=905000 },
    { make="Lucid", model="Air", ingame="Lucaid", year="2024", rarity="🟡 Semi-Rare", pct="0.5%", minP=455555, maxP=465000 },
    { make="Tesla", model="Cybertruck", ingame="Syper", year="2024", rarity="🟡 Semi-Rare", pct="0.5%", minP=455555, maxP=465000 },
    { make="Mercedes-Benz", model="G-Class", ingame="G-Cross", year="2019", rarity="🟡 Semi-Rare", pct="0.5%", minP=342000, maxP=355000 },
    { make="Bentley", model="Flying Spur", ingame="Bentarra", year="2023", rarity="🟢 Uncommon", pct="1.5%", minP=1150000, maxP=1200000 },
    { make="Lexus", model="LX600", ingame="Lexira", year="2024", rarity="🟢 Uncommon", pct="1.5%", minP=555555, maxP=570000 },
    { make="Lexus", model="LX600", ingame="Lexira", year="2024", rarity="🟢 Uncommon", pct="1.5%", minP=555555, maxP=570000 },
    { make="Mercedes-Benz", model="AMG GT-63 S", ingame="jT Class", year="2021", rarity="🟢 Uncommon", pct="2%", minP=520000, maxP=580000 },
    { make="Audi", model="RS8", ingame="Audlra", year="2021", rarity="🟢 Uncommon", pct="2%", minP=558500, maxP=565000 },
    { make="BMW", model="M5", ingame="N5", year="2021", rarity="🔵 Moderate", pct="3%", minP=548500, maxP=555000 },
    { make="Land Rover", model="Defender 110", ingame="ديفندا", year="2023", rarity="🔵 Moderate", pct="3%", minP=485000, maxP=505000 },
    { make="Lexus", model="LX570", ingame="Lexira", year="2015", rarity="🔵 Moderate", pct="3%", minP=231333, maxP=244444 },
    { make="BMW", model="M8", ingame="C8", year="2024", rarity="🔵 Moderate", pct="4%", minP=670000, maxP=680000 },
    { make="BMW", model="M4", ingame="N4", year="2024", rarity="🔵 Moderate", pct="4%", minP=595000, maxP=601100 },
    { make="Toyota", model="Supra", ingame="Supry", year="2023", rarity="🔵 Moderate", pct="4%", minP=175000, maxP=181000 },
    { make="Porsche", model="Cayenne", ingame="Caeenr", year="2022", rarity="🔵 Moderate", pct="5%", minP=410000, maxP=420000 },
    { make="Lexus", model="LX", ingame="Lexira", year="2020", rarity="🔵 Moderate", pct="5%", minP=375000, maxP=385000 },
    { make="Chevrolet", model="Corvette C8", ingame="Corver", year="2020", rarity="🔵 Moderate", pct="5%", minP=360000, maxP=375000 },
    { make="Dodge", model="Charger Hellcat", ingame="Charjero Helksa", year="2021", rarity="🔵 Moderate", pct="5%", minP=332000, maxP=354000 },
    { make="Lexus", model="LS", ingame="Lexira", year="2022", rarity="🔵 Moderate", pct="5%", minP=293000, maxP=308000 },
    { make="Chevrolet", model="Corvette C6 ZR1", ingame="Corver", year="2010", rarity="🔵 Moderate", pct="5%", minP=290000, maxP=305000 },
    { make="Genesis", model="G70", ingame="Genera J70", year="2022", rarity="🔵 Moderate", pct="5%", minP=160000, maxP=183580 },
    { make="Lexus", model="RX350", ingame="Lexira RX 350", year="2024", rarity="⚪ Common", pct="5.5%", minP=289000, maxP=312000 },
    { make="Lexus", model="IS350", ingame="Lexira IS 350", year="2023", rarity="⚪ Common", pct="5.5%", minP=232000, maxP=245000 },
    { make="Lexus", model="LC500", ingame="Lexira", year="2024", rarity="⚪ Common", pct="7%", minP=351000, maxP=362000 },
    { make="Chrysler", model="300 SRT", ingame="Christo SRT", year="2024", rarity="⚪ Common", pct="7%", minP=192000, maxP=205000 },
    { make="Mercedes-Benz", model="AMG C63", ingame="T Class", year="2017", rarity="⚪ Common", pct="7%", minP=165000, maxP=178000 },
    { make="Hyundai", model="Azera", ingame="AZORA", year="2021", rarity="⚪ Common", pct="7%", minP=88000, maxP=95000 },
    { make="Hyundai", model="Azera", ingame="AZORA", year="2021", rarity="⚪ Common", pct="7%", minP=88000, maxP=95000 },
    { make="Hyundai", model="Azera", ingame="AZORA", year="2021", rarity="⚪ Common", pct="7%", minP=88000, maxP=95000 },
    { make="Audi", model="S4", ingame="Aura S4", year="2025", rarity="⚪ Common", pct="7.5%", minP=219981, maxP=234731 },
    { make="Dodge", model="RAM TRX", ingame="Raf TRX", year="2024", rarity="⚪ Common", pct="8%", minP=390000, maxP=410000 },
    { make="Toyota", model="Land Cruiser", ingame="Crucer", year="2023", rarity="⚪ Common", pct="8%", minP=310000, maxP=320000 },
    { make="Hyundai", model="Azera", ingame="Azora", year="2024", rarity="⚪ Common", pct="8%", minP=174000, maxP=184000 },
    { make="Chevrolet", model="Tahoe", ingame="Tahero", year="2025", rarity="⚪ Common", pct="9%", minP=410000, maxP=420000 },
    { make="GMC", model="Yukon", ingame="Yakon", year="2023", rarity="⚪ Common", pct="9%", minP=231000, maxP=241000 },
    { make="BMW", model="M2", ingame="N2", year="2023", rarity="⚪ Common", pct="9%", minP=220000, maxP=232000 },
    { make="GMC", model="Sierra", ingame="Sierro", year="2021", rarity="⚪ Common", pct="9%", minP=172000, maxP=185000 },
    { make="Ford", model="Mustang Shelby", ingame="Shilpa", year="2007", rarity="⚪ Common", pct="9%", minP=145000, maxP=155000 },
    { make="Toyota", model="Land Cruiser", ingame="Cruzer", year="2023", rarity="⚪ Common", pct="9%", minP=143000, maxP=155000 },
    { make="BMW", model="Series 3", ingame="Ceres 3", year="2020", rarity="⚪ Common", pct="9%", minP=138000, maxP=152000 },
    { make="Genesis", model="G90", ingame="J70", year="2020", rarity="⚪ Common", pct="10%", minP=135000, maxP=145000 },
    { make="Ford", model="Taurus", ingame="توري", year="2025", rarity="⬜ Very Common", pct="13%", minP=132000, maxP=148000 },
    { make="Dodge", model="Charger GT", ingame="Charjero GT", year="2020", rarity="⬜ Very Common", pct="13%", minP=110000, maxP=125000 },
    { make="Chevrolet", model="Tahoe LS", ingame="Tahiro", year="2019", rarity="⬜ Very Common", pct="13%", minP=93000, maxP=107000 },
    { make="Lexus", model="ES", ingame="Lexira AS", year="2022", rarity="⬜ Very Common", pct="14%", minP=175000, maxP=185000 },
    { make="Toyota", model="Camry", ingame="Kimora", year="2020", rarity="⬜ Very Common", pct="14%", minP=79000, maxP=88888 },
    { make="Ford", model="F150 Raptor", ingame="Rafter F15", year="2025", rarity="⬜ Very Common", pct="15%", minP=397000, maxP=410000 },
    { make="Nissan", model="Patrol", ingame="Patrel", year="2025", rarity="⬜ Very Common", pct="15%", minP=292000, maxP=305000 },
    { make="GMC", model="Sierra", ingame="Siora", year="2024", rarity="⬜ Very Common", pct="15%", minP=263000, maxP=273000 },
    { make="Toyota", model="Land Cruiser", ingame="لاند", year="2020", rarity="⬜ Very Common", pct="15%", minP=212000, maxP=222000 },
    { make="Toyota", model="Avaol", ingame="Avalyn", year="2022", rarity="⬜ Very Common", pct="15%", minP=133000, maxP=155000 },
    { make="Kia", model="Cadenza", ingame="Kadi", year="2022", rarity="⬜ Very Common", pct="15%", minP=132000, maxP=145000 },
    { make="Toyota", model="Camry", ingame="Kimora", year="2025", rarity="⬜ Very Common", pct="15%", minP=138340, maxP=143070 },
    { make="Toyota", model="Camry", ingame="Kimora", year="2023", rarity="⬜ Very Common", pct="15%", minP=129000, maxP=141000 },
    { make="Kia", model="K5", ingame="C5", year="2025", rarity="⬜ Very Common", pct="15%", minP=115000, maxP=125000 },
    { make="Ford", model="Raptor", ingame="Raptor", year="2017", rarity="⬜ Very Common", pct="15%", minP=115000, maxP=125000 },
    { make="Hyundai", model="Kona", ingame="Kora", year="2024", rarity="⬜ Very Common", pct="15%", minP=98520, maxP=108000 },
    { make="Kia", model="K5", ingame="C5", year="2025", rarity="⬜ Very Common", pct="15%", minP=95000, maxP=105000 },
    { make="Hyundai", model="Sonata", ingame="Sonira", year="2025", rarity="⬜ Very Common", pct="15%", minP=95000, maxP=105000 },
    { make="BMW", model="M4", ingame="N4", year="2015", rarity="⬜ Very Common", pct="15%", minP=94000, maxP=105000 },
    { make="Chevrolet", model="Camaro", ingame="Ramarro", year="2017", rarity="⬜ Very Common", pct="15%", minP=65000, maxP=77000 },
    { make="Toyota", model="Sequoia", ingame="Sqora", year="2011", rarity="⬜ Very Common", pct="15%", minP=67000, maxP=75000 },
    { make="Kia", model="Optima", ingame="Optoma", year="2019", rarity="⬜ Very Common", pct="15%", minP=65000, maxP=68000 },
    { make="Toyota", model="Avalon", ingame="Avalyn", year="2012", rarity="⬜ Very Common", pct="15%", minP=45555, maxP=55555 },
    { make="Chevrolet", model="Tahoe", ingame="Tahiro", year="2013", rarity="⬜ Very Common", pct="15%", minP=48000, maxP=54000 },
    { make="Toyota", model="Land Cruiser", ingame="Crucer", year="2023", rarity="⬜ Very Common", pct="19%", minP=310000, maxP=320000 },
    { make="Toyota", model="Hilux", ingame="Helix", year="2023", rarity="⬜ Very Common", pct="19%", minP=128000, maxP=133000 },
    { make="Dodge", model="Charger SXT", ingame="Charjero", year="2020", rarity="⬜ Very Common", pct="19%", minP=45000, maxP=54000 },
    { make="Honda", model="Accord", ingame="Akura", year="2024", rarity="⬜ Very Common", pct="20%", minP=132885, maxP=145000 },
    { make="Toyota", model="Camry", ingame="Kimora", year="2025", rarity="⬜ Very Common", pct="20%", minP=118340, maxP=123070 },
    { make="Hyundai", model="Elantra", ingame="Alanter", year="2025", rarity="⬜ Very Common", pct="20%", minP=105000, maxP=112000 },
    { make="Chery", model="Arrizo 8", ingame="Ariza 8", year="2023", rarity="⬜ Very Common", pct="20%", minP=106000, maxP=112000 },
    { make="Nissan", model="Maxima", ingame="Maxina", year="2023", rarity="⬜ Very Common", pct="20%", minP=91000, maxP=102000 },
    { make="Toyota", model="Camry", ingame="Cemora", year="2021", rarity="⬜ Very Common", pct="20%", minP=88000, maxP=95000 },
    { make="Toyota", model="Camry", ingame="Cemora", year="2021", rarity="⬜ Very Common", pct="20%", minP=88000, maxP=95000 },
    { make="GAC Motor", model="EMPOW", ingame="Empora", year="2024", rarity="⬜ Very Common", pct="20%", minP=66000, maxP=81000 },
    { make="MG Motor", model="GT", ingame="MGera GT", year="2022", rarity="📦 Super Common", pct="21%", minP=64975, maxP=69975 },
    { make="Toyota", model="Yaris", ingame="يارسر", year="2024", rarity="📦 Super Common", pct="22%", minP=65000, maxP=75000 },
    { make="GMC", model="Sierra", ingame="Serao", year="2021", rarity="📦 Super Common", pct="25%", minP=175000, maxP=185000 },
    { make="Honda", model="Accord", ingame="Akura", year="2024", rarity="📦 Super Common", pct="25%", minP=121000, maxP=130950 },
    { make="Hyundai", model="Azera", ingame="Azeora", year="2018", rarity="📦 Super Common", pct="25%", minP=115000, maxP=128000 },
    { make="GMC", model="Sierra", ingame="Searo", year="2018", rarity="📦 Super Common", pct="25%", minP=115000, maxP=128000 },
    { make="Toyota", model="Avalon", ingame="Avalyn", year="2019", rarity="📦 Super Common", pct="25%", minP=115000, maxP=128000 },
    { make="Kia", model="K8", ingame="Q8", year="2023", rarity="📦 Super Common", pct="25%", minP=115000, maxP=125000 },
    { make="Hyundai", model="Sonata", ingame="Sonira", year="2024", rarity="📦 Super Common", pct="25%", minP=112000, maxP=122000 },
    { make="Hyundai", model="Sonata", ingame="Sonira", year="2025", rarity="📦 Super Common", pct="25%", minP=95000, maxP=105000 },
    { make="Hyundai", model="Elantra", ingame="Alanter", year="2025", rarity="📦 Super Common", pct="25%", minP=85000, maxP=92000 },
    { make="Hyundai", model="Elantra", ingame="Alanter", year="2025", rarity="📦 Super Common", pct="25%", minP=75000, maxP=82000 },
    { make="Kia", model="Sportage", ingame="Sportana", year="2023", rarity="📦 Super Common", pct="25%", minP=73000, maxP=82000 },
    { make="Hyundai", model="Elantra", ingame="Alanter", year="2018", rarity="📦 Super Common", pct="25%", minP=67555, maxP=75777 },
    { make="Kia", model="Cadenza", ingame="Cadi", year="2017", rarity="📦 Super Common", pct="25%", minP=65555, maxP=75555 },
    { make="Toyota", model="RAV4", ingame="Titan-X", year="2021", rarity="📦 Super Common", pct="25%", minP=65000, maxP=75000 },
    { make="Hyundai", model="Sonata", ingame="Sonera", year="2020", rarity="📦 Super Common", pct="25%", minP=64000, maxP=74000 },
    { make="Toyota", model="Aurion", ingame="Aurian", year="2012", rarity="📦 Super Common", pct="25%", minP=65000, maxP=72000 },
    { make="Honda", model="Accord", ingame="Acora", year="2017", rarity="📦 Super Common", pct="25%", minP=45555, maxP=65555 },
    { make="Hyundai", model="Sonata", ingame="Sonera", year="2013", rarity="📦 Super Common", pct="25%", minP=53000, maxP=62000 },
    { make="Geely", model="Emgrand", ingame="Jelira Emgrand", year="2023", rarity="📦 Super Common", pct="25%", minP=48000, maxP=62000 },
    { make="Toyota", model="Camry", ingame="Kimora", year="2016", rarity="📦 Super Common", pct="25%", minP=49000, maxP=61000 },
    { make="Chevrolet", model="Caprice", ingame="Capas", year="2011", rarity="📦 Super Common", pct="25%", minP=45555, maxP=55555 },
    { make="Kia", model="Cadenza", ingame="Cadi", year="2015", rarity="📦 Super Common", pct="25%", minP=45555, maxP=55555 },
    { make="Chevrolet", model="Caprice", ingame="Capas", year="2008", rarity="📦 Super Common", pct="25%", minP=45555, maxP=55555 },
    { make="Toyota", model="Camry", ingame="Kemora", year="2011", rarity="📦 Super Common", pct="25%", minP=35000, maxP=42000 },
    { make="Toyota", model="Aurion", ingame="Aurioz", year="2007", rarity="📦 Super Common", pct="25%", minP=25000, maxP=35000 },
    { make="Hyundai", model="Elantra", ingame="Alantar", year="2020", rarity="📦 Super Common", pct="27%", minP=51000, maxP=61000 },
    { make="Kia", model="Optim", ingame="Optira", year="2019", rarity="📦 Super Common", pct="28%", minP=35000, maxP=42000 },
    { make="Nissan", model="Ddseb", ingame="Dedsona", year="2016", rarity="📦 Super Common", pct="29%", minP=33000, maxP=42000 },
    { make="Chevrolet", model="Cruze", ingame="Craoz", year="2025", rarity="📦 Super Common", pct="30%", minP=78000, maxP=81000 },
    { make="Toyota", model="Camry", ingame="Cemora", year="2006", rarity="📦 Super Common", pct="30%", minP=25000, maxP=32000 },
    { make="Lexus", model="LS 420", ingame="Lexira LS 430", year="2001", rarity="📦 Super Common", pct="30%", minP=19000, maxP=28000 },
    { make="Lexus", model="LS 600h", ingame="Lexira", year="2008", rarity="📦📦 Extremely Common", pct="33%", minP=35000, maxP=42000 },
    { make="Ford", model="Taurus", ingame="Fusior", year="2010", rarity="📦📦 Extremely Common", pct="35%", minP=47000, maxP=51000 },
    { make="Ford", model="Fusion", ingame="Fusior", year="2010", rarity="📦📦 Extremely Common", pct="35%", minP=39000, maxP=42000 },
    { make="Audi", model="A4", ingame="Audira A4", year="2013", rarity="📦📦 Extremely Common", pct="35%", minP=24000, maxP=31000 },
    { make="Toyota", model="Camry", ingame="Cemora", year="2004", rarity="📦📦 Extremely Common", pct="35%", minP=16000, maxP=22000 },
    { make="Chevrolet", model="Cruze", ingame="Krause", year="2015", rarity="📦📦 Extremely Common", pct="40%", minP=17000, maxP=22000 },
    { make="Nissan", model="Sunny", ingame="Solira", year="2024", rarity="📦📦 Extremely Common", pct="45%", minP=45680, maxP=55000 },
    { make="Nissan", model="Altima", ingame="Altira", year="2008", rarity="📦📦 Extremely Common", pct="45%", minP=25000, maxP=31333 },
}

local function parseGameName(rawName)
    if not rawName or rawName == "" then return "", nil end
    -- Strip variant suffix like _2 _3
    local s = rawName:gsub("_%d+$", "")
    -- Try 4-digit year first (20xx / 19xx)
    local year4 = s:match("(20%d%d)") or s:match("(19%d%d)")
    local yearNum = year4 and tonumber(year4) or nil
    -- Try 2-digit year suffix (e.g. "Sonata24" -> 2024)
    if not yearNum then
        local y2 = s:match("(%d%d)$")
        if y2 then
            local n = tonumber(y2)
            if n and n <= 30 then yearNum = 2000 + n
            elseif n and n >= 90 then yearNum = 1900 + n
            end
        end
    end
    -- Base name: strip all digits, keep letters only, lowercase
    local base = s:gsub("%d+", ""):gsub("[%s%-%_]", ""):lower()
    return base, yearNum
end

local function scoreEntry(entry, baseName, yearNum, slotPrice)
    local score = 0
    local modelL  = entry.model:lower():gsub("[%s%-%_]",""):gsub("%d+","")
    local ingameL  = entry.ingame:lower():gsub("[%s%-%_]",""):gsub("%d+","")
    local makeL   = entry.make:lower():gsub("[%s%-%_]","")

    if baseName == "" then return 0 end

    -- Model name scoring
    if modelL == baseName then score = score + 100
    elseif #baseName >= 4 and modelL:find(baseName, 1, true) then score = score + 65
    elseif #baseName >= 4 and baseName:find(modelL, 1, true) then score = score + 55
    elseif #baseName >= 3 then
        -- Partial prefix match (e.g. "elantr" matches "elantra")
        local shorter = #baseName < #modelL and baseName or modelL
        local longer  = #baseName < #modelL and modelL or baseName
        if longer:sub(1, #shorter) == shorter then score = score + 45 end
    end

    -- In-game name scoring (fallback for cars like Kimora/Lexira)
    if score == 0 then
        if ingameL == baseName then score = score + 80
        elseif #baseName >= 4 and ingameL:find(baseName, 1, true) then score = score + 50
        elseif #baseName >= 4 and baseName:find(ingameL, 1, true) then score = score + 40
        end
    end

    -- Make scoring (very weak, only if model/ingame gave nothing)
    if score == 0 and makeL:find(baseName, 1, true) then score = score + 15 end

    -- Year bonus
    if yearNum and entry.year ~= "" then
        local dbYear = tonumber(entry.year)
        if dbYear == yearNum then score = score + 30
        elseif dbYear and math.abs(dbYear - yearNum) <= 1 then score = score + 12
        end
    end

    -- Price proximity bonus
    if slotPrice and slotPrice > 0 and (entry.minP > 0 or entry.maxP > 0) then
        local mid = (entry.minP + entry.maxP) / 2
        if mid > 0 then
            local ratio = slotPrice / mid
            if   ratio >= 0.80 and ratio <= 1.25 then score = score + 30
            elseif ratio >= 0.60 and ratio <= 1.60 then score = score + 12
            end
        end
    end

    return score
end

local function dbLookup(rawName, slotPrice)
    local baseName, yearNum = parseGameName(rawName)
    if baseName == "" then return nil end

    local best, bestScore = nil, 0
    for _, entry in ipairs(CAR_DB) do
        local s = scoreEntry(entry, baseName, yearNum, slotPrice)
        if s > bestScore then best = entry; bestScore = s end
    end

    -- Must have at least a name match (score >= 20) to return
    return bestScore >= 20 and best or nil, bestScore
end


local function makeResultCard(scrollFrame, rankLabel, priceStr, plateText, plateNum, slotName, carData, accentColor, dbInfo)
    -- Clean card: rank, rarity, price, car name, plate/slot, TP button
    local card = Instance.new("Frame", scrollFrame)
    card.Size = UDim2.new(1, -6, 0, 88)
    card.BackgroundColor3 = Color3.fromRGB(20, 20, 30)
    card.BorderSizePixel = 0
    Instance.new("UICorner", card).CornerRadius = UDim.new(0, 7)
    Instance.new("UIStroke", card).Color = Color3.fromRGB(38, 38, 58)

    -- Top accent bar
    local topBar = Instance.new("Frame", card)
    topBar.Size = UDim2.new(1, 0, 0, 2)
    topBar.BackgroundColor3 = accentColor
    topBar.BorderSizePixel = 0
    Instance.new("UICorner", topBar).CornerRadius = UDim.new(0, 7)

    -- Row 1: rank (left) | rarity (right)
    local rank = Instance.new("TextLabel", card)
    rank.Size = UDim2.new(0.5, 0, 0, 14)
    rank.Position = UDim2.new(0, 4, 0, 4)
    rank.Text = rankLabel
    rank.TextColor3 = accentColor
    rank.Font = Enum.Font.GothamBold
    rank.TextSize = 9
    rank.BackgroundTransparency = 1
    rank.TextXAlignment = Enum.TextXAlignment.Left

    if dbInfo and not dbInfo._noMatch then
        local rarL = Instance.new("TextLabel", card)
        rarL.Size = UDim2.new(0.5, -4, 0, 14)
        rarL.Position = UDim2.new(0.5, 0, 0, 4)
        rarL.Text = dbInfo.rarity
        rarL.TextColor3 = Color3.fromRGB(160, 160, 205)
        rarL.Font = Enum.Font.Gotham
        rarL.TextSize = 8
        rarL.BackgroundTransparency = 1
        rarL.TextXAlignment = Enum.TextXAlignment.Right
        rarL.TextTruncate = Enum.TextTruncate.AtEnd
    end

    -- Row 2: price (left) | car name (right)
    local priceL = Instance.new("TextLabel", card)
    priceL.Size = UDim2.new(0.52, 0, 0, 18)
    priceL.Position = UDim2.new(0, 4, 0, 19)
    priceL.Text = "$" .. priceStr
    priceL.TextColor3 = Color3.fromRGB(255, 215, 50)
    priceL.Font = Enum.Font.GothamBold
    priceL.TextSize = 14
    priceL.BackgroundTransparency = 1
    priceL.TextXAlignment = Enum.TextXAlignment.Left

    if dbInfo then
        local nameL = Instance.new("TextLabel", card)
        nameL.Size = UDim2.new(0.46, 0, 0, 18)
        nameL.Position = UDim2.new(0.53, 0, 0, 19)
        if dbInfo._noMatch then
            nameL.Text = tostring(dbInfo._rawName or "?")
            nameL.TextColor3 = Color3.fromRGB(160, 130, 70)
        else
            nameL.Text = dbInfo.make .. " " .. dbInfo.model
            nameL.TextColor3 = Color3.fromRGB(140, 190, 255)
        end
        nameL.Font = Enum.Font.GothamBold
        nameL.TextSize = 8
        nameL.BackgroundTransparency = 1
        nameL.TextXAlignment = Enum.TextXAlignment.Right
        nameL.TextTruncate = Enum.TextTruncate.AtEnd
    end

    -- Row 3: plate | slot
    local plateL = Instance.new("TextLabel", card)
    plateL.Size = UDim2.new(1, -8, 0, 13)
    plateL.Position = UDim2.new(0, 4, 0, 39)
    plateL.Text = plateText .. "  |  " .. plateNum .. "  |  " .. slotName
    plateL.TextColor3 = TEXT_GRAY
    plateL.Font = Enum.Font.Gotham
    plateL.TextSize = 8
    plateL.BackgroundTransparency = 1
    plateL.TextXAlignment = Enum.TextXAlignment.Left
    plateL.TextTruncate = Enum.TextTruncate.AtEnd

    -- Row 4: TP button
    local tpBtn = Instance.new("TextButton", card)
    tpBtn.Size = UDim2.new(1, -8, 0, 18)
    tpBtn.Position = UDim2.new(0, 4, 0, 56)
    tpBtn.Text = "▶  TELEPORT TO DEALER"
    tpBtn.BackgroundColor3 = Color3.fromRGB(15, 35, 65)
    tpBtn.TextColor3 = Color3.fromRGB(90, 160, 255)
    tpBtn.Font = Enum.Font.GothamBold
    tpBtn.TextSize = 9
    tpBtn.BorderSizePixel = 0
    Instance.new("UICorner", tpBtn).CornerRadius = UDim.new(0, 4)
    Instance.new("UIStroke", tpBtn).Color = Color3.fromRGB(35, 75, 145)

    tpBtn.MouseButton1Click:Connect(function()
        if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            player.Character.HumanoidRootPart.CFrame = carData.cf
        end
    end)

    return card
end


local function clearScrolls()
    for _, c in ipairs(leftScroll:GetChildren()) do
        if not c:IsA("UIListLayout") then c:Destroy() end
    end
    for _, c in ipairs(rightScroll:GetChildren()) do
        if not c:IsA("UIListLayout") then c:Destroy() end
    end
end

local function addEmptyNote(scrollFrame, msg, col)
    local lbl = Instance.new("TextLabel", scrollFrame)
    lbl.Size = UDim2.new(1, -6, 0, 40)
    lbl.Text = msg
    lbl.TextColor3 = col or TEXT_GRAY
    lbl.Font = Enum.Font.GothamBold
    lbl.TextSize = 10
    lbl.BackgroundTransparency = 1
    lbl.TextWrapped = true
end

local scanRunning = false

deepScanStopBtn.MouseButton1Click:Connect(function()
    if scanRunning then
        scanRunning = false
        deepScanStopBtn.Text = "✓"
        task.wait(0.8)
        deepScanStopBtn.Text = "■"
    end
end)

local function runScan(slowMode)
    if scanRunning then return end
    scanRunning = true

    local locs = {
        Vector3.new(-2730, 6, 3294),  Vector3.new(-2718, 6, 3505),
        Vector3.new(-1385, 4, -958),  Vector3.new(-1672, 4, -967),
        Vector3.new(-1677, 4, -670),  Vector3.new(-1324, 4, -671),
        Vector3.new(-1386, 4, -374),  Vector3.new(-1651, 4, -368),
        Vector3.new(-1908, 4, -277),  Vector3.new(-1918, 5, 15),
        Vector3.new(-1677, 4, -67),   Vector3.new(-1356, 4, -68),
        Vector3.new(-1919, 5, 170)
    }

    -- For deep scan: slower per-zone wait, and repeat passes until 50+ cars found (max 3 passes)
    local waitCap      = slowMode and 5.0 or scanSpeed   -- seconds per zone cap
    local stableNeeded = slowMode and 5     or 3          -- stable frames before moving on
    local maxPasses    = slowMode and 3     or 1
    local minTarget    = 50

    -- Lock both buttons
    scanBtn.Active        = false
    deepScanBtn.Active    = false
    scanBtnGrad.Enabled   = false
    deepScanBtnGrad.Enabled = false
    scanBtn.BackgroundColor3      = Color3.fromRGB(40, 40, 60)
    deepScanBtn.BackgroundColor3  = Color3.fromRGB(40, 25, 70)
    clearScrolls()

    local foundData = {}
    local passNum   = 0

    repeat
        passNum += 1
        if slowMode then
            statusBar.Text = "🔍 Deep scan pass " .. passNum .. "/" .. maxPasses .. "..."
        end

        for i, pos in ipairs(locs) do
            if not scanRunning then break end
            local root = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
            if not root then break end

            local label = slowMode
                and ("🔍 PASS " .. passNum .. "  " .. i .. "/" .. #locs)
                or  ("📡 SCANNING " .. i .. "/" .. #locs)

            if slowMode then
                deepScanBtn.Text = label
            else
                scanBtn.Text = label
            end
            statusBar.Text = (slowMode and "Deep " or "") .. "Scan zone " .. i .. "/" .. #locs
                .. (passNum > 1 and ("  (pass " .. passNum .. ")") or "") .. "..."

            root.CFrame = CFrame.new(pos)
            if i == 1 and passNum == 1 then task.wait(1.4) end

            -- Adaptive wait until slot count stabilises
            local slots_check = workspace:FindFirstChild("Slots")
            if slots_check then
                local lastCount    = -1
                local stableFrames = 0
                local elapsed      = 0
                while stableFrames < stableNeeded and elapsed < waitCap do
                    local count = #slots_check:GetChildren()
                    if count == lastCount then stableFrames += 1
                    else stableFrames = 0; lastCount = count end
                    task.wait(0.1)
                    elapsed += 0.1
                end
            else
                task.wait(waitCap)
            end

            local slots = workspace:FindFirstChild("Slots")
            if slots then
                for _, slot in ipairs(slots:GetChildren()) do
                    local price   = slot:GetAttribute("Price")
                    local licText = slot:GetAttribute("LicenseText")   or "N/A"
                    local licNum  = slot:GetAttribute("LicenseNumber") or "N/A"
                    local carName = ""
                    for _, attrName in ipairs({"CarName","VehicleName","Model","ModelName","Car","Name","DisplayName","CarModel","Vehicle"}) do
                        local v = slot:GetAttribute(attrName)
                        if v and tostring(v) ~= "" then carName = tostring(v); break end
                    end
                    local dealer = slot:FindFirstChild("Dealer")
                    if carName == "" and dealer then carName = dealer.Name or "" end
                    if price and tonumber(price) > 0 and dealer then
                        local cf = dealer:IsA("Model") and dealer:GetPivot() or dealer.CFrame
                        foundData[slot.Name] = {
                            name        = slot.Name,
                            price       = tonumber(price),
                            cf          = cf,
                            licenseText = tostring(licText),
                            licenseNum  = tostring(licNum),
                            carName     = carName,
                        }
                    end
                end
            end
        end

        -- Count after this pass
        local currentCount = 0
        for _ in pairs(foundData) do currentCount += 1 end

        if slowMode and currentCount < minTarget and passNum < maxPasses then
            statusBar.Text = "⚠ Only " .. currentCount .. " cars found — retrying pass " .. (passNum+1) .. "..."
            task.wait(1)
        end

    until (not slowMode) or passNum >= maxPasses or (function()
        local n = 0; for _ in pairs(foundData) do n += 1 end; return n >= minTarget
    end)()

    -- Build results
    local allCars = {}
    for _, d in pairs(foundData) do table.insert(allCars, d) end
    table.sort(allCars, function(a, b) return a.price > b.price end)

    clearScrolls()

    -- LEFT: top 3
    local rankLabels = { "#1  TOP PRICE", "#2  2ND PLACE", "#3  3RD PLACE" }
    local rankColors = {
        Color3.fromRGB(0, 200, 255),
        Color3.fromRGB(160, 160, 200),
        Color3.fromRGB(200, 140, 60)
    }
    local shown = math.min(3, #allCars)
    if shown == 0 then
        addEmptyNote(leftScroll, "No cars found.", TEXT_GRAY)
    else
        for i = 1, shown do
            local car = allCars[i]
            local dbInfo, dbScore = dbLookup(car.carName, car.price)
            if not dbInfo and car.carName ~= "" then
                dbInfo = { make=car.carName, model="", year="?", rarity="❓ Unknown", pct="?", minP=0, maxP=0, _noMatch=true, _rawName=car.carName }
            elseif dbInfo then
                dbInfo = setmetatable({_score=dbScore, _rawName=car.carName}, {__index=dbInfo})
            end
            makeResultCard(leftScroll, rankLabels[i], formatNumber(car.price),
                car.licenseText, car.licenseNum, car.name, car, rankColors[i], dbInfo)
        end
    end

    -- RIGHT: all special plates
    local specials = {}
    for _, car in ipairs(allCars) do
        if isSpecialPlate(car.licenseText, car.licenseNum) then
            table.insert(specials, car)
        end
    end
    if #specials == 0 then
        addEmptyNote(rightScroll, "No special plates found.", Color3.fromRGB(200, 160, 0))
    else
        for i, car in ipairs(specials) do
            local dbInfo, dbScore = dbLookup(car.carName, car.price)
            if not dbInfo and car.carName ~= "" then
                dbInfo = { make=car.carName, model="", year="?", rarity="❓ Unknown", pct="?", minP=0, maxP=0, _noMatch=true, _rawName=car.carName }
            elseif dbInfo then
                dbInfo = setmetatable({_score=dbScore, _rawName=car.carName}, {__index=dbInfo})
            end
            makeResultCard(rightScroll, "⭐ #"..i, formatNumber(car.price),
                car.licenseText, car.licenseNum, car.name, car, Color3.fromRGB(220,180,0), dbInfo)
        end
    end

    local totalFound = #allCars
    local suffix = slowMode and (" in " .. passNum .. " pass" .. (passNum>1 and "es" or "")) or ""
    local countColor = (slowMode and totalFound < minTarget) and Color3.fromRGB(255,160,60) or ACCENT_GRN
    statusBar.Text = totalFound .. " cars found" .. suffix .. ",  " .. #specials .. " special."
    statusBar.TextColor3 = countColor

    scanBtn.Text         = "📡 SCAN"
    deepScanBtn.Text     = "🔍 DEEP SCAN"
    deepScanStopBtn.Text = "■"
    scanBtnGrad.Enabled  = true
    deepScanBtnGrad.Enabled = true
    scanBtn.BackgroundColor3     = ACCENT
    deepScanBtn.BackgroundColor3 = Color3.fromRGB(80, 40, 160)
    scanBtn.Active       = true
    deepScanBtn.Active   = true
    scanRunning = false
end

scanBtn.MouseButton1Click:Connect(function()
    task.spawn(runScan, false)
end)

deepScanBtn.MouseButton1Click:Connect(function()
    task.spawn(runScan, true)
end)

-- ===========================
-- LOGIC: INSTANT UPGRADE (all-in-one with money-check retry)
-- Performance first (fast 0.3s spam), cosmetics last (2s cooldown each)
-- Each purchase is confirmed by checking if money decreased.
-- ===========================
local modRunning = false
modStopBtn.MouseButton1Click:Connect(function()
    modRunning = false
    modStopBtn.Text = "✓"
    task.wait(0.8)
    modStopBtn.Text = "■"
end)
modBtn.MouseButton1Click:Connect(function()
    if modRunning then return end
    modRunning = true
    modBtn.Active = false
    modBtnGrad.Enabled = false
    modBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 55)

    task.spawn(function()

    local moneyVal = player:FindFirstChild("leaderstats")
        and player.leaderstats:FindFirstChild("Money")

    local function getMoney()
        return moneyVal and moneyVal.Value or 0
    end

    -- Detect which garage platform we're near so we can validate ownership
    local function detectGaragePos()
        local root = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
        if not root then return nil end
        for _, pd in ipairs(GARAGE_PLATFORMS) do
            if (root.Position - pd.cf.Position).Magnitude < 20 then
                return pd.pos
            end
        end
        return nil
    end
    local currentGaragePos = detectGaragePos()

    -- Retries buyFn until money drops or maxRetries; pauses if platform taken
    local function buyUntilPurchased(label, buyFn, cooldown, maxRetries)
        cooldown   = cooldown   or 0
        maxRetries = maxRetries or 8  -- reduced: if 8 attempts don't move money, upgrade is owned/unavailable
        local moneyBefore = getMoney()
        local attempts    = 0
        repeat
            if not modRunning then break end  -- respect stop
            -- Garage validator: pause if platform is taken
            if currentGaragePos then
                while not afGarageOurs(currentGaragePos) do
                    modBtn.Text = "⏸ Waiting for platform..."
                    statusBar.Text = "Platform taken — waiting..."
                    task.wait(1)
                    local r = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
                    if r then r.CFrame = CFrame.new(currentGaragePos) end
                    task.wait(0.8)
                    pcall(function() PromptGarageRemote:FireServer() end)
                    task.wait(1.5)
                end
            end
            attempts += 1
            modBtn.Text    = label .. "  (" .. attempts .. ")"
            statusBar.Text = label .. " — attempt " .. attempts
            pcall(buyFn)
            for _=1,6 do if not modRunning then break end task.wait(0.05) end
        until getMoney() < moneyBefore or attempts >= maxRetries or not modRunning

        if getMoney() < moneyBefore then
            statusBar.Text = "✓ " .. label
        elseif attempts >= maxRetries then
            statusBar.Text = "⏭ " .. label .. " — skipped (already owned or unavailable)"
        end
        if cooldown > 0 and modRunning then task.wait(cooldown) end
    end

    -- ── PERFORMANCE (fast, no extra cooldown) ──
    if modUpgrades.Fix then
        modBtn.Text = "🔧 Fixing car..."
        statusBar.Text = "Fixing car..."
        pcall(function() FixRemote:InvokeServer() end)
        task.wait(0.3)
    end

    if modUpgrades.Accel1 then buyUntilPurchased("🏎 AccelerationLvl 1/3", function() Remote:InvokeServer("Install", { section = "AccelerationLvl", level = 1 }) end, 0) end
    if modUpgrades.Accel2 then buyUntilPurchased("🏎 AccelerationLvl 2/3", function() Remote:InvokeServer("Install", { section = "AccelerationLvl", level = 2 }) end, 0) end
    if modUpgrades.Accel3 then buyUntilPurchased("🏎 AccelerationLvl 3/3", function() Remote:InvokeServer("Install", { section = "AccelerationLvl", level = 3 }) end, 0) end

    if modUpgrades.Engine1 then buyUntilPurchased("⚙ EngineLvl 1/2", function() Remote:InvokeServer("Install", { section = "EngineLvl", level = 1 }) end, 0) end
    if modUpgrades.Engine2 then buyUntilPurchased("⚙ EngineLvl 2/2", function() Remote:InvokeServer("Install", { section = "EngineLvl", level = 2 }) end, 0) end

    if modUpgrades.Nitro1 then buyUntilPurchased("💨 NitroLvl 1/3", function() Remote:InvokeServer("Install", { section = "NitroLvl", level = 1 }) end, 0) end
    if modUpgrades.Nitro2 then buyUntilPurchased("💨 NitroLvl 2/3", function() Remote:InvokeServer("Install", { section = "NitroLvl", level = 2 }) end, 0) end
    if modUpgrades.Nitro3 then buyUntilPurchased("💨 NitroLvl 3/3", function() Remote:InvokeServer("Install", { section = "NitroLvl", level = 3 }) end, 0) end

    if modUpgrades.Paint then
        buyUntilPurchased("🖌 Paint (Black)", function()
            Remote:InvokeServer("Install", { section="Paint", upgradeName="Black", requestedColors={Color1=Color3.new(0.157,0.157,0.157)} })
        end, 0)
    end

    if modUpgrades.Light then
        buyUntilPurchased("💡 LightColor (Black)", function()
            Remote:InvokeServer("Install", { section="LightColor", upgradeName="Black", requestedColors={Color1=Color3.new(0.157,0.157,0.157)} })
        end, 0)
    end

    -- ── COSMETICS (2s cooldown) ──
    if modUpgrades.NitroCol then
        buyUntilPurchased("🔥 NitroColor (JetFlame)", function()
            Remote:InvokeServer("Install", { section="NitroColor", upgradeName="JetFlame" })
        end, 2)
    end

    if modUpgrades.Underglow then
        buyUntilPurchased("🌈 Underglow", function()
            Remote:InvokeServer("Install", { section="Underglow", upgradeName="TwoColorFrontBackSplit",
                requestedColors={Secondary=Color3.new(0.82,0.54,0.25), Primary=Color3.new(0.23,0.50,0.78)} })
        end, 2)
    end

    modBtn.Text = modRunning and "✅ ALL DONE" or "⏹ Stopped"
    statusBar.Text = modRunning and "Upgrade complete!" or "Stopped."
    task.wait(1.5)
    modRunning = false
    modBtn.Text = "⚡ INSTANT UPGRADE CAR"
    modBtnGrad.Enabled = true
    modBtn.BackgroundColor3 = ACCENT_GRN
    modBtn.Active = true
    statusBar.Text = "Ready"

    end) -- close task.spawn
end)

-- ===========================
-- LOGIC: BUY COSMETICS (stub — merged into INSTANT UPGRADE above)
-- ===========================
cosBtn.MouseButton1Click:Connect(function() end)

-- ===========================
-- LOGIC: AUTO CAR CALCULATOR
-- Reads player.Cars, sums BoughtPrice + CheckPrice + FixPrice + MoneySpent
-- Auto-detects level via player:GetAttribute("Level")
-- ===========================
local lastRawPrice = 0

local function getPlayerLevel()
    -- Try attribute first, then NumberValue child
    local attrLevel = player:GetAttribute("Level")
    if attrLevel then return tonumber(attrLevel) or 1 end
    local lvlVal = player:FindFirstChild("Level")
    if lvlVal and lvlVal:IsA("NumberValue") then return lvlVal.Value end
    -- Also check leaderstats
    local ls = player:FindFirstChild("leaderstats")
    if ls then
        local lv = ls:FindFirstChild("Level")
        if lv then return tonumber(lv.Value) or 1 end
    end
    return 1
end

local function calcMargin(level)
    return 0.05 + (math.floor(level / 5) * 0.02)
end

local function buildCarList()
    -- Clear existing cards (destroy everything except the layout)
    for _, c in ipairs(carScroll:GetChildren()) do
        if not c:IsA("UIListLayout") then c:Destroy() end
    end

    local level  = getPlayerLevel()
    local margin = calcMargin(level)
    levelDisplay.Text = "Level: " .. level .. "  |  Margin: " .. string.format("%.0f", margin * 100) .. "%"

    local carsFolder = player:FindFirstChild("Cars")
    if not carsFolder then
        local noCarLbl = Instance.new("TextLabel", carScroll)
        noCarLbl.Size = UDim2.new(1, 0, 0, 40)
        noCarLbl.Text = "⚠ player.Cars not found"
        noCarLbl.TextColor3 = Color3.fromRGB(255, 80, 80)
        noCarLbl.Font = Enum.Font.GothamBold
        noCarLbl.TextSize = 11
        noCarLbl.BackgroundTransparency = 1
        return
    end

    -- Collect all car data first so we can sort
    local carData = {}
    for _, carFolder in ipairs(carsFolder:GetChildren()) do
        local carName    = carFolder:GetAttribute("CarName")   or carFolder.Name
        local bought     = tonumber(carFolder:GetAttribute("BoughtPrice"))  or 0
        local checkP     = tonumber(carFolder:GetAttribute("CheckPrice"))   or 0
        local fixP       = tonumber(carFolder:GetAttribute("FixPrice"))     or 0
        local moneySpent = tonumber(carFolder:GetAttribute("MoneySpent"))   or 0
        local totalInvested = bought + checkP + fixP + moneySpent
        local sellPrice     = math.floor(totalInvested + totalInvested * margin)
        local profit        = sellPrice - totalInvested

        -- Hide zero filter
        if hideZeroEnabled and moneySpent == 0 then continue end

        table.insert(carData, {
            folderName    = carFolder.Name,  -- UUID string, needed for sell button
            name          = carName,
            bought        = bought,
            checkP        = checkP,
            fixP          = fixP,
            moneySpent    = moneySpent,
            totalInvested = totalInvested,
            sellPrice     = sellPrice,
            profit        = profit,
        })
    end

    if #carData == 0 then
        local noCarLbl = Instance.new("TextLabel", carScroll)
        noCarLbl.Size = UDim2.new(1, 0, 0, 40)
        noCarLbl.Text = hideZeroEnabled and "No cars with spending found." or "No cars found."
        noCarLbl.TextColor3 = TEXT_GRAY
        noCarLbl.Font = Enum.Font.GothamBold
        noCarLbl.TextSize = 11
        noCarLbl.BackgroundTransparency = 1
        return
    end

    -- Sort highest sell price first
    table.sort(carData, function(a, b) return a.sellPrice > b.sellPrice end)

    for idx, car in ipairs(carData) do
        -- Card
        local card = Instance.new("Frame", carScroll)
        card.Size = UDim2.new(1, 0, 0, 178)  -- taller to fit three button rows + cancel
        card.BackgroundColor3 = BG_PANEL
        card.BorderSizePixel = 0
        card.LayoutOrder = idx
        Instance.new("UICorner", card).CornerRadius = UDim.new(0, 8)

        local topAccent = Instance.new("Frame", card)
        topAccent.Size = UDim2.new(1, 0, 0, 2)
        topAccent.BackgroundColor3 = Color3.fromRGB(200, 120, 0)
        topAccent.BorderSizePixel = 0
        Instance.new("UICorner", topAccent).CornerRadius = UDim.new(0, 8)

        -- Car name
        local nameL = Instance.new("TextLabel", card)
        nameL.Size = UDim2.new(1, -10, 0, 20)
        nameL.Position = UDim2.new(0, 8, 0, 4)
        nameL.Text = "🚗 " .. car.name
        nameL.TextColor3 = TEXT_WHITE
        nameL.Font = Enum.Font.GothamBold
        nameL.TextSize = 13
        nameL.BackgroundTransparency = 1
        nameL.TextXAlignment = Enum.TextXAlignment.Left

        local function smallLbl(txt, xScale, yOff, col)
            local l = Instance.new("TextLabel", card)
            l.Size = UDim2.new(0.48, 0, 0, 14)
            l.Position = UDim2.new(xScale, 4, 0, yOff)
            l.Text = txt
            l.TextColor3 = col or TEXT_GRAY
            l.Font = Enum.Font.Gotham
            l.TextSize = 10
            l.BackgroundTransparency = 1
            l.TextXAlignment = Enum.TextXAlignment.Left
        end

        smallLbl("Bought:  $" .. formatNumber(car.bought),     0,    26)
        smallLbl("Check:   $" .. formatNumber(car.checkP),     0.5,  26)
        smallLbl("Fix:     $" .. formatNumber(car.fixP),       0,    40)
        smallLbl("Spent:   $" .. formatNumber(car.moneySpent), 0.5,  40)

        local div = Instance.new("Frame", card)
        div.Size = UDim2.new(0.92, 0, 0, 1)
        div.Position = UDim2.new(0.04, 0, 0, 57)
        div.BackgroundColor3 = Color3.fromRGB(45, 45, 60)
        div.BorderSizePixel = 0

        local totalL = Instance.new("TextLabel", card)
        totalL.Size = UDim2.new(0.55, 0, 0, 16)
        totalL.Position = UDim2.new(0, 8, 0, 60)
        totalL.Text = "Total in: $" .. formatNumber(car.totalInvested)
        totalL.TextColor3 = Color3.fromRGB(180, 180, 200)
        totalL.Font = Enum.Font.GothamBold
        totalL.TextSize = 10
        totalL.BackgroundTransparency = 1
        totalL.TextXAlignment = Enum.TextXAlignment.Left

        local profitL = Instance.new("TextLabel", card)
        profitL.Size = UDim2.new(0.45, 0, 0, 16)
        profitL.Position = UDim2.new(0.55, 0, 0, 60)
        profitL.Text = "Profit: $" .. formatNumber(car.profit)
        profitL.TextColor3 = Color3.fromRGB(80, 255, 140)
        profitL.Font = Enum.Font.GothamBold
        profitL.TextSize = 10
        profitL.BackgroundTransparency = 1
        profitL.TextXAlignment = Enum.TextXAlignment.Left

        local sellL = Instance.new("TextLabel", card)
        sellL.Size = UDim2.new(0.6, 0, 0, 20)
        sellL.Position = UDim2.new(0, 8, 0, 78)
        sellL.Text = "SELL FOR: $" .. formatNumber(car.sellPrice)
        sellL.TextColor3 = Color3.fromRGB(255, 215, 50)
        sellL.Font = Enum.Font.GothamBold
        sellL.TextSize = 13
        sellL.BackgroundTransparency = 1
        sellL.TextXAlignment = Enum.TextXAlignment.Left

        local cardCopy = Instance.new("TextButton", card)
        cardCopy.Size = UDim2.new(0.44, 0, 0, 22)
        cardCopy.Position = UDim2.new(0, 8, 0, 78)
        cardCopy.Text = "📋 COPY"
        cardCopy.BackgroundColor3 = Color3.fromRGB(50, 50, 65)
        cardCopy.TextColor3 = TEXT_WHITE
        cardCopy.Font = Enum.Font.GothamBold
        cardCopy.TextSize = 10
        cardCopy.BorderSizePixel = 0
        Instance.new("UICorner", cardCopy).CornerRadius = UDim.new(0, 5)

        local cardSell = Instance.new("TextButton", card)
        cardSell.Size = UDim2.new(0.44, 0, 0, 22)
        cardSell.Position = UDim2.new(0.52, 0, 0, 78)
        cardSell.Text = "🏷 LIST FOR SALE"
        cardSell.BackgroundColor3 = Color3.fromRGB(160, 80, 0)
        cardSell.TextColor3 = TEXT_WHITE
        cardSell.Font = Enum.Font.GothamBold
        cardSell.TextSize = 9
        cardSell.BorderSizePixel = 0
        Instance.new("UICorner", cardSell).CornerRadius = UDim.new(0, 5)

        -- List at -3% margin button
        local discountPrice = math.floor(car.sellPrice * 0.97)
        local cardSellDisc = Instance.new("TextButton", card)
        cardSellDisc.Size = UDim2.new(0.9, 0, 0, 22)
        cardSellDisc.Position = UDim2.new(0, 8, 0, 104)
        cardSellDisc.Text = "📉 LIST −3%  →  $" .. formatNumber(discountPrice)
        cardSellDisc.BackgroundColor3 = Color3.fromRGB(80, 40, 120)
        cardSellDisc.TextColor3 = Color3.fromRGB(200, 160, 255)
        cardSellDisc.Font = Enum.Font.GothamBold
        cardSellDisc.TextSize = 9
        cardSellDisc.BorderSizePixel = 0
        Instance.new("UICorner", cardSellDisc).CornerRadius = UDim.new(0, 5)
        Instance.new("UIStroke", cardSellDisc).Color = Color3.fromRGB(110, 60, 180)

        -- Cancel listing button
        local cardCancel = Instance.new("TextButton", card)
        cardCancel.Size = UDim2.new(0.9, 0, 0, 22)
        cardCancel.Position = UDim2.new(0, 8, 0, 130)
        cardCancel.Text = "🚫 CANCEL LISTING"
        cardCancel.BackgroundColor3 = Color3.fromRGB(65, 18, 18)
        cardCancel.TextColor3 = Color3.fromRGB(255, 100, 100)
        cardCancel.Font = Enum.Font.GothamBold
        cardCancel.TextSize = 9
        cardCancel.BorderSizePixel = 0
        Instance.new("UICorner", cardCancel).CornerRadius = UDim.new(0, 5)
        Instance.new("UIStroke", cardCancel).Color = Color3.fromRGB(130, 35, 35)

        -- Status label for sell result
        local sellStatus = Instance.new("TextLabel", card)
        sellStatus.Size = UDim2.new(1, -16, 0, 18)
        sellStatus.Position = UDim2.new(0, 8, 0, 156)
        sellStatus.Text = ""
        sellStatus.TextColor3 = ACCENT_GRN
        sellStatus.Font = Enum.Font.GothamBold
        sellStatus.TextSize = 10
        sellStatus.BackgroundTransparency = 1
        sellStatus.TextXAlignment = Enum.TextXAlignment.Left

        local copySellPrice = car.sellPrice
        local copyCarName   = car.name
        local carUUID       = car.folderName  -- UUID like "{...}"

        cardCopy.MouseButton1Click:Connect(function()
            if setclipboard then
                setclipboard(tostring(copySellPrice))
                cardCopy.Text             = "✓ COPIED"
                cardCopy.BackgroundColor3 = ACCENT_GRN
                task.wait(1.5)
                cardCopy.Text             = "📋 COPY"
                cardCopy.BackgroundColor3 = Color3.fromRGB(50, 50, 65)
            end
        end)

        cardSell.MouseButton1Click:Connect(function()
            cardSell.Active = false
            cardSell.Text   = "⏳ Listing..."
            local pcarsF = workspace:FindFirstChild("PlayersCars")
            local cModel = pcarsF and pcarsF:FindFirstChild(carUUID)
            if not cModel then
                sellStatus.Text       = "⚠ Car not spawned in world"
                sellStatus.TextColor3 = Color3.fromRGB(255, 80, 80)
                cardSell.Text         = "🏷 LIST FOR SALE"
                cardSell.Active       = true
                return
            end
            local ok, err = pcall(function()
                PromptSellRemote:InvokeServer("ListVehicle", cModel, copySellPrice, false)
            end)
            if ok then
                sellStatus.Text           = "✅ Listed at $" .. formatNumber(copySellPrice)
                sellStatus.TextColor3     = ACCENT_GRN
                cardSell.Text             = "✓ LISTED"
                cardSell.BackgroundColor3 = Color3.fromRGB(0, 100, 0)
            else
                sellStatus.Text       = "⚠ " .. tostring(err):sub(1, 45)
                sellStatus.TextColor3 = Color3.fromRGB(255, 80, 80)
                cardSell.Text         = "🏷 LIST FOR SALE"
                cardSell.Active       = true
            end
        end)

        cardSellDisc.MouseButton1Click:Connect(function()
            cardSellDisc.Active = false
            cardSellDisc.Text   = "⏳ Listing..."
            local pcarsF = workspace:FindFirstChild("PlayersCars")
            local cModel = pcarsF and pcarsF:FindFirstChild(carUUID)
            if not cModel then
                sellStatus.Text       = "⚠ Car not spawned in world"
                sellStatus.TextColor3 = Color3.fromRGB(255, 80, 80)
                cardSellDisc.Text     = "📉 LIST −3%  →  $" .. formatNumber(discountPrice)
                cardSellDisc.Active   = true
                return
            end
            local ok, err = pcall(function()
                PromptSellRemote:InvokeServer("ListVehicle", cModel, discountPrice, false)
            end)
            if ok then
                sellStatus.Text             = "✅ Listed at $" .. formatNumber(discountPrice) .. " (−3%)"
                sellStatus.TextColor3       = Color3.fromRGB(180, 120, 255)
                cardSellDisc.Text           = "✓ LISTED −3%"
                cardSellDisc.BackgroundColor3 = Color3.fromRGB(60, 30, 100)
            else
                sellStatus.Text       = "⚠ " .. tostring(err):sub(1, 45)
                sellStatus.TextColor3 = Color3.fromRGB(255, 80, 80)
                cardSellDisc.Text     = "📉 LIST −3%  →  $" .. formatNumber(discountPrice)
                cardSellDisc.Active   = true
            end
        end)
        cardCancel.MouseButton1Click:Connect(function()
            cardCancel.Active = false
            cardCancel.Text   = "⏳ Cancelling..."
            local pcarsF = workspace:FindFirstChild("PlayersCars")
            local cModel = pcarsF and pcarsF:FindFirstChild(carUUID)
            if not cModel then
                sellStatus.Text       = "⚠ Car not in world"
                sellStatus.TextColor3 = Color3.fromRGB(255, 80, 80)
                cardCancel.Text       = "🚫 CANCEL LISTING"
                cardCancel.Active     = true
                return
            end
            local ok, err = pcall(function()
                PromptSellRemote:InvokeServer("CancelListing", cModel)
            end)
            if ok then
                sellStatus.Text             = "✅ Listing cancelled"
                sellStatus.TextColor3       = Color3.fromRGB(255, 140, 140)
                cardCancel.Text             = "✓ CANCELLED"
                cardCancel.BackgroundColor3 = Color3.fromRGB(40, 10, 10)
            else
                sellStatus.Text       = "⚠ " .. tostring(err):sub(1, 45)
                sellStatus.TextColor3 = Color3.fromRGB(255, 80, 80)
                cardCancel.Text       = "🚫 CANCEL LISTING"
                cardCancel.Active     = true
            end
        end)
    end  -- close for loop
end  -- close buildCarList

hideZeroBtn.MouseButton1Click:Connect(function()
    hideZeroEnabled = not hideZeroEnabled
    if hideZeroEnabled then
        hideZeroBtn.Text             = "🚫 HIDE $0 CARS"
        hideZeroBtn.BackgroundColor3 = Color3.fromRGB(140, 0, 0)
        hideZeroBtn.TextColor3       = TEXT_WHITE
    else
        hideZeroBtn.Text             = "👁 SHOW ALL"
        hideZeroBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 65)
        hideZeroBtn.TextColor3       = TEXT_GRAY
    end
    buildCarList()
end)

refreshBtn.MouseButton1Click:Connect(function()
    refreshBtn.Text = "..."
    buildCarList()
    refreshBtn.Text = "🔄 REFRESH"
end)

-- Auto-build when switching to calc tab
tabCalcBtn.MouseButton1Click:Connect(function()
    buildCarList()
end)

-- ===========================
-- LOGIC: AUTOFARM
-- ===========================
local AF = {
    running   = false,
    autoloop  = false,
    step      = "IDLE",
    logCount  = 0,
    -- Resume state: filled when a cycle partially completes
    saved = {
        active       = false,
        newCarFolder = nil,
        carModel     = nil,
        listPrice    = 0,
        desk         = nil,
        phase        = "",
    },
}

local BargainRemote      = ReplicatedStorage:WaitForChild("Remotes"):WaitForChild("Baragin")
local PromptSaleRemote   = ReplicatedStorage:WaitForChild("Remotes"):WaitForChild("PromptSale")
local PromptGarageRemote = ReplicatedStorage:WaitForChild("Remotes"):WaitForChild("PromptGarage")
local FetchSpawnCarsRemote = ReplicatedStorage:WaitForChild("Remotes"):WaitForChild("FetchSpawnCars")
local ShowVHCStatsRemote = ReplicatedStorage:WaitForChild("Remotes"):WaitForChild("ShowVHCStats")

-- ── SHARED GARAGE PLATFORM DATA (declared at top of file) ──

-- Returns the Platform instance that matches a given garagePos, or nil
local function afFindPlatformObj(garagePos)
    local gf = workspace:FindFirstChild("Garage_Contents")
        and workspace.Garage_Contents:FindFirstChild("GaragePlatforms")
    if not gf then return nil end
    for _, plat in ipairs(gf:GetChildren()) do
        local pp = plat:IsA("BasePart") and plat.Position
            or (plat:IsA("Model") and plat.PrimaryPart and plat.PrimaryPart.Position)
        if pp and (pp - Vector3.new(garagePos.X, pp.Y, garagePos.Z)).Magnitude < 6 then
            return plat
        end
    end
    return nil
end

-- Returns true if the platform at garagePos belongs to us
local function afGarageOurs(garagePos)
    local plat = afFindPlatformObj(garagePos)
    if not plat then return true end -- can't check = assume ok
    local own = tostring(plat:GetAttribute("Owner") or "")
    return own == "" or own == tostring(player.UserId) or own == player.Name
end

-- Stronger check: confirms garage UI is actually OPEN (Owner must be set TO us, not just empty)
local function afGarageIsOpen(garagePos)
    local plat = afFindPlatformObj(garagePos)
    if not plat then return false end
    local own = tostring(plat:GetAttribute("Owner") or "")
    return own == tostring(player.UserId) or own == player.Name
end

-- Block until garage is ours again (reteleports player to platform using autofarm method)
-- Returns false if AF.running was set to false while waiting
local function afWaitForGarage(garagePos)
    while not afGarageOurs(garagePos) do
        if not AF.running then return false end
        -- Log once every 3s
        task.wait(3)
        if afGarageOurs(garagePos) then break end
        -- TP player back to the platform
        local r = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
        if r then r.CFrame = CFrame.new(garagePos) end
        task.wait(0.8)
        pcall(function() PromptGarageRemote:FireServer() end)
        task.wait(1.5)
    end
    return AF.running
end

local function afLog(msg, col)
    AF.logCount += 1
    local line = Instance.new("TextLabel", farmLogScroll)
    line.Size = UDim2.new(1, -6, 0, 16)
    line.Text = "[" .. AF.logCount .. "] " .. msg
    line.TextColor3 = col or TEXT_GRAY
    line.Font = Enum.Font.Gotham
    line.TextSize = 10
    line.BackgroundTransparency = 1
    line.TextXAlignment = Enum.TextXAlignment.Left
    line.LayoutOrder = AF.logCount
    farmStepLabel.Text = "Step: " .. msg
    statusBar.Text     = msg
end

local function afGetMoney()
    local moneyVal = player:FindFirstChild("leaderstats")
        and player.leaderstats:FindFirstChild("Money")
    return moneyVal and moneyVal.Value or 0
end

-- Wait up to timeout seconds for condition fn to return true
local function afWait(condFn, timeout, interval)
    interval = interval or 0.2
    local elapsed = 0
    while elapsed < timeout do
        if not AF.running then return false end
        if condFn() then return true end
        task.wait(interval)
        elapsed += interval
    end
    return false
end

-- Snapshot car IDs currently in player.Cars
local function afSnapshotCars()
    local snap = {}
    local folder = player:FindFirstChild("Cars")
    if folder then
        for _, c in ipairs(folder:GetChildren()) do
            snap[c.Name] = true
        end
    end
    return snap
end

-- Find new car folder after purchase
local function afFindNewCar(before)
    local folder = player:FindFirstChild("Cars")
    if not folder then return nil end
    for _, c in ipairs(folder:GetChildren()) do
        if not before[c.Name] then return c end
    end
    return nil
end

-- ── RECOVER CAR: teleport to fetch point, call FetchSpawnCars, drive car back to plot ──
-- Called when car disappears / is sent to void before listing
local FETCH_POS = Vector3.new(-1231, 6, 4804)
local function afRecoverCar(carFolder, plotCF)
    if not carFolder then return false end
    local carUUID = carFolder.Name

    afLog("🔧 Car missing — attempting recovery...", Color3.fromRGB(255, 160, 50))

    -- 1. TP player to fetch position
    local root = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
    if root then root.CFrame = CFrame.new(FETCH_POS) end
    task.wait(1)

    -- 2. Fire FetchSpawnCars with the car UUID
    afLog("  Calling FetchSpawnCars for " .. carUUID)
    pcall(function() FetchSpawnCarsRemote:FireServer(carUUID) end)
    task.wait(2.5)  -- wait for car to appear near plot

    -- 3. Find the car in PlayersCars
    local cModel = workspace:FindFirstChild("PlayersCars") and workspace.PlayersCars:FindFirstChild(carUUID)
    if not cModel then
        afLog("  ⚠ Car still not found after fetch", Color3.fromRGB(255, 80, 80))
        return false
    end

    -- 4. Teleport player to the car
    root = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
    local ok, carPivot = pcall(function() return cModel:GetPivot() end)
    if root and ok then
        root.CFrame = carPivot + Vector3.new(0, 3, 0)
        task.wait(0.5)
    end

    -- 5. Enter the car
    local entered = afEnterCar(cModel)
    if entered then
        afLog("  Seated in recovered car ✓", ACCENT_GRN)
    else
        afLog("  ⚠ Could not enter recovered car — attempting CFrame TP")
    end
    task.wait(0.5)

    -- 6. Drive (TP) car + player to plot
    if plotCF then
        root = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
        if root then root.CFrame = plotCF + Vector3.new(0, 3, 0) end
        task.wait(0.5)
        -- Re-find car after TP
        cModel = workspace:FindFirstChild("PlayersCars") and workspace.PlayersCars:FindFirstChild(carUUID)
    end

    afLog("  Car recovered ✓", ACCENT_GRN)
    return true
end

-- Get current car model in workspace.PlayersCars for a given car folder
local function afGetCarModel(carFolder)
    local id = carFolder.Name  -- UUID string like "{...}"
    return workspace:FindFirstChild("PlayersCars") and
           workspace.PlayersCars:FindFirstChild(id)
end

-- Try to sit the player in a car model
local function afEnterCar(carModel)
    if not carModel then return false end
    local char = player.Character
    if not char then return false end
    local hum  = char:FindFirstChildOfClass("Humanoid")
    if not hum  then return false end
    -- Try VehicleSeat first
    local seat = carModel:FindFirstChildOfClass("VehicleSeat")
        or carModel:FindFirstChildWhichIsA("Seat", true)
    if seat then
        pcall(function() seat:Sit(hum) end)
        task.wait(0.5)
        if hum.SeatPart then return true end
    end
    -- Try firing any ProximityPrompt in the car
    for _, desc in ipairs(carModel:GetDescendants()) do
        if desc:IsA("ProximityPrompt") then
            pcall(function() fireproximityprompt(desc) end)
            task.wait(0.5)
            if hum.SeatPart then return true end
        end
    end
    return hum.SeatPart ~= nil
end

-- Exit car by pressing E / triggering exit
local function afExitCar(carModel)
    local char = player.Character
    local hum  = char and char:FindFirstChildOfClass("Humanoid")
    if not hum or not hum.SeatPart then return true end
    -- Try Inputs folder in the car model
    if carModel then
        local inputs = carModel:FindFirstChild("Inputs", true)
        if inputs then
            for _, v in ipairs(inputs:GetChildren()) do
                pcall(function()
                    if v:IsA("BindableEvent") then v:Fire() end
                    if v:IsA("RemoteEvent")   then v:FireServer() end
                end)
            end
        end
    end
    -- Force unseat
    pcall(function() hum.Sit = false end)
    task.wait(0.4)
    return hum.SeatPart == nil
end

-- Teleport player/car to position
-- afTeleportTo removed (unused)

-- Try to close the Desktop PC GUI
local function afClosePC(deskRef)
    -- First try: re-fire the same PC ProximityPrompt that opened it (acts as toggle)
    if deskRef then
        local pp = deskRef:FindFirstChild("MainSeat", true)
        pp = pp and pp:FindFirstChildOfClass("ProximityPrompt")
        if pp then
            pcall(function() fireproximityprompt(pp) end)
            task.wait(0.6)
        end
        -- Also search any ProximityPrompt in the desk
        for _, desc in ipairs(deskRef:GetDescendants()) do
            if desc:IsA("ProximityPrompt") then
                pcall(function() fireproximityprompt(desc) end)
                task.wait(0.4)
            end
        end
    end
    -- Second try: fire the Shutdown button signal
    pcall(function()
        local desktop = player.PlayerGui:FindFirstChild("Desktop")
        if not desktop then return end
        local shutdown = desktop:FindFirstChild("Shutdown", true)
        if shutdown then
            pcall(function() firesignal(shutdown.MouseButton1Click) end)
            pcall(function() shutdown.MouseButton1Click:Fire() end)
        end
    end)
    -- Third try: destroy the Desktop gui entirely if still open
    pcall(function()
        local desktop = player.PlayerGui:FindFirstChild("Desktop")
        if desktop and desktop.Enabled then
            desktop.Enabled = false
        end
    end)
    task.wait(0.8)
end

-- Full autofarm single cycle
local function afRunCycle()
    -- Parse shorthand: 100k, 1.4m, 1m, 300000, etc.
    local function parsePrice(txt)
        txt = tostring(txt or ""):lower():gsub(",",""):gsub("%s","")
        local num = tonumber(txt)
        if num then return num end
        local val, suf = txt:match("^([%d%.]+)([km])$")
        if val and suf then
            num = tonumber(val) or 0
            return suf == "k" and num * 1000 or num * 1000000
        end
        return 300000
    end
    local minPrice = parsePrice(farmPriceInput.Text)

    -- ── STEP 1: SCAN for cars above threshold (collect ALL, sorted best first) ──
    afLog("🔍 Scanning for cars > $" .. formatNumber(minPrice), Color3.fromRGB(100, 180, 255))
    local locs = {
        Vector3.new(-163, 7, -17),    Vector3.new(-164, 7, -240),
        Vector3.new(-1453, 6, 5353),  Vector3.new(-1497, 6, 4968),
        Vector3.new(-1600, 6, 4567),  Vector3.new(-1656, 6, 4149),
        Vector3.new(-1044, 6, 5524),  Vector3.new(-1113, 6, 5444),
        Vector3.new(-1292, 6, 4270)
    }

    local candidateCars = {}  -- all cars above threshold, best price first

    for i, pos in ipairs(locs) do
        if not AF.running then return end
        afLog("  Zone " .. i .. "/" .. #locs)
        local root = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
        if not root then afLog("⚠ No character!", Color3.fromRGB(255,80,80)) return end
        root.CFrame = CFrame.new(pos)

        -- Adaptive wait
        local slotsF = workspace:FindFirstChild("Slots")
        if slotsF then
            local last, stable, elapsed = -1, 0, 0
            while stable < 3 and elapsed < 3 do
                local cnt = #slotsF:GetChildren()
                stable = (cnt == last) and (stable + 1) or 0
                last = cnt
                task.wait(0.15)
                elapsed += 0.15
            end
        else
            task.wait(2)
        end

        local slots = workspace:FindFirstChild("Slots")
        if slots then
            for _, slot in ipairs(slots:GetChildren()) do
                local price  = tonumber(slot:GetAttribute("Price") or 0)
                local dealer = slot:FindFirstChild("Dealer")
                if price >= minPrice and dealer then
                    local cf = dealer:IsA("Model") and dealer:GetPivot() or dealer.CFrame
                    -- Avoid duplicates (same slot from multiple zones)
                    local exists = false
                    for _, c in ipairs(candidateCars) do
                        if c.slotName == slot.Name then exists = true; break end
                    end
                    if not exists then
                        table.insert(candidateCars, { slotObj = slot, slotName = slot.Name, price = price, cf = cf, pos = pos })
                    end
                end
            end
        end
    end

    -- Sort best price first
    table.sort(candidateCars, function(a, b) return a.price > b.price end)

    if #candidateCars == 0 then
        afLog("❌ No car found above $" .. formatNumber(minPrice), Color3.fromRGB(255, 100, 100))
        return
    end
    afLog("✅ Found " .. #candidateCars .. " candidate(s)", Color3.fromRGB(80, 255, 140))
    if not AF.running then return end

    -- ── STEPS 2-3: TRY EACH CANDIDATE (best price first) — skip if dealer busy ──
    local bestCar    = nil
    local slotFolder = nil
    local myName     = player.Name

    for idx, candidate in ipairs(candidateCars) do
        if not AF.running then return end
        afLog("🚶 Trying #" .. idx .. ": " .. candidate.slotName .. " $" .. formatNumber(candidate.price), Color3.fromRGB(180, 220, 255))

        local root = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
        if not root then afLog("⚠ No character", Color3.fromRGB(255,80,80)) return end

        root.CFrame = candidate.cf + Vector3.new(0, 3, 0)
        task.wait(1.5)

        local dist = (root.Position - candidate.cf.Position).Magnitude
        if dist > 30 then
            root.CFrame = candidate.cf + Vector3.new(0, 3, 0)
            task.wait(1.5)
        end

        local sf = workspace:FindFirstChild("Slots") and workspace.Slots:FindFirstChild(candidate.slotName)
        local skipThis = false

        -- Pre-check: dealer already occupied?
        if sf then
            local currentUser = tostring(sf:GetAttribute("CurrentUser") or "")
            if currentUser ~= "" and currentUser ~= myName then
                afLog("  ⏭ Dealer busy (CurrentUser='" .. currentUser .. "') — skipping to next", Color3.fromRGB(255, 180, 0))
                skipThis = true
            end
        end

        -- ── STEP 3: INTERACT ──
        if not skipThis and sf then
            afLog("🤝 Interacting with NPC dealer...")
            local interacted = false
            local function tryFireAll()
                local npcSlot = sf:FindFirstChild("NpcSlot", true)
                if npcSlot then
                    local interaction = npcSlot:FindFirstChild("Interaction")
                    if interaction then
                        local pp = interaction:IsA("ProximityPrompt") and interaction
                            or interaction:FindFirstChildOfClass("ProximityPrompt")
                        if pp then pcall(function() fireproximityprompt(pp) end) end
                    end
                end
                for _, desc in ipairs(sf:GetDescendants()) do
                    if desc:IsA("ProximityPrompt") then pcall(function() fireproximityprompt(desc) end) end
                end
            end

            for attempt = 1, 8 do
                tryFireAll()
                task.wait(0.7)
                local cu = tostring(sf:GetAttribute("CurrentUser") or "")
                if cu == myName then
                    interacted = true
                    afLog("  Confirmed ✓ (CurrentUser=" .. cu .. ")", ACCENT_GRN)
                    break
                elseif cu ~= "" and cu ~= myName then
                    afLog("  Dealer taken by '" .. cu .. "' — skipping", Color3.fromRGB(255,180,0))
                    break
                end
                afLog("  Attempt " .. attempt .. " — CurrentUser='" .. cu .. "'")
            end

            if interacted then
                bestCar    = candidate
                slotFolder = sf
                break  -- success — proceed with this car
            else
                afLog("  ⚠ Interaction failed — trying next candidate", Color3.fromRGB(255,80,80))
            end
        end
    end

    if not bestCar then
        afLog("❌ All candidates busy or failed — aborting", Color3.fromRGB(255, 80, 80))
        return
    end

    -- Wait 2.3s after interaction so the NPC dialogue fully loads
    afLog("  Waiting for NPC dialogue...", TEXT_GRAY)
    task.wait(2.3)
    if not AF.running then return end

    -- Fire ShowVHCStats NOW (after NPC dialogue is open) — makes seller lower price
    afLog("📊 Checking car stats (ShowVHCStats)...", Color3.fromRGB(180, 180, 255))
    pcall(function() ShowVHCStatsRemote:FireServer(slotFolder) end)
    task.wait(0.8)
    if not AF.running then return end

    -- ── STEP 4: BARGAIN (-6% → -4%) then BUY ──
    afLog("💬 Bargaining with seller...")
    local price6 = math.floor(bestCar.price * 0.94)   -- -6% off listed price
    local price4 = math.floor(bestCar.price * 0.96)   -- -4% off listed price

    if slotFolder then
        afLog("  Offer 1: $" .. formatNumber(price6) .. " (−6%)")
        pcall(function() BargainRemote:FireServer(price6, slotFolder) end)
        task.wait(1.5)
        if not AF.running then return end

        afLog("  Offer 2: $" .. formatNumber(price4) .. " (−4%)")
        pcall(function() BargainRemote:FireServer(price4, slotFolder) end)
        task.wait(1.5)
        if not AF.running then return end
    end

    afLog("💳 Buying car...")
    local carsBefore = afSnapshotCars()
    moneyBefore = afGetMoney()

    pcall(function() PromptSaleRemote:FireServer("Auth", slotFolder) end)

    -- Wait for new car to appear in player.Cars
    local newCarFolder = nil
    local bought = afWait(function()
        newCarFolder = afFindNewCar(carsBefore)
        return newCarFolder ~= nil
    end, 8)

    if not bought or not newCarFolder then
        afLog("❌ Buy failed — car not detected", Color3.fromRGB(255,80,80))
        return
    end
    afLog("✅ Bought: " .. (newCarFolder:GetAttribute("CarName") or newCarFolder.Name), ACCENT_GRN)

    -- Save state so resume can skip scan/buy if we fail later
    AF.saved.active       = true
    AF.saved.newCarFolder = newCarFolder
    AF.saved.phase        = "upgrade"
    farmResumeBar.Visible = true
    task.wait(1)
    if not AF.running then return end

    -- ── STEP 6: ENTER CAR ──
    afLog("🚗 Entering car...")
    local carModel = nil
    afWait(function()
        carModel = afGetCarModel(newCarFolder)
        return carModel ~= nil
    end, 6)

    local inCar = false
    if carModel then
        inCar = afEnterCar(carModel)
        if inCar then
            afLog("  Seated in car ✓", ACCENT_GRN)
        else
            afLog("  ⚠ Couldn't sit — continuing on foot")
        end
    else
        afLog("  ⚠ Car model not found in workspace")
    end
    task.wait(0.5)
    if not AF.running then return end

    -- ── STEP 7: PICK GARAGE PLATFORM (check Owner attribute) ──
    afLog("🏠 Checking garage platforms...")

    -- Use shared GARAGE_PLATFORMS table defined above
    local chosenPlatform = nil
    local garagePlatformsFolder = workspace:FindFirstChild("Garage_Contents")
        and workspace.Garage_Contents:FindFirstChild("GaragePlatforms")

    if garagePlatformsFolder then
        for _, plat in ipairs(garagePlatformsFolder:GetChildren()) do
            local owner = plat:GetAttribute("Owner")
            local platPos = plat:IsA("BasePart") and plat.Position
                or (plat:IsA("Model") and plat.PrimaryPart and plat.PrimaryPart.Position)
            if platPos then
                for _, pd in ipairs(GARAGE_PLATFORMS) do
                    if (platPos - pd.cf.Position).Magnitude < 6 then
                        local isFree = (not owner) or (owner == "") or (owner == player.UserId) or (tostring(owner) == tostring(player.UserId)) or (tostring(owner) == player.Name)
                        if isFree and not chosenPlatform then
                            chosenPlatform = pd
                            afLog("  " .. pd.label .. " is free ✓", ACCENT_GRN)
                        elseif not isFree then
                            afLog("  " .. pd.label .. " occupied (Owner: " .. tostring(owner) .. ")")
                        end
                        break
                    end
                end
            end
        end
    end
    if not chosenPlatform then
        chosenPlatform = GARAGE_PLATFORMS[1]
        afLog("  ⚠ Platform check failed — defaulting to A")
    end

    local garagePos = chosenPlatform.pos

    afLog("🏠 Teleporting to " .. chosenPlatform.label .. "...")
    smartTeleport(CFrame.new(garagePos))
    task.wait(1)

    -- ── STEP 8: OPEN GARAGE — retry until Owner attribute confirms it's open ──
    afLog("🔧 Opening garage...")
    local garageOpenConfirmed = false
    local garageOpenAttempts  = 0
    local maxGarageAttempts   = 8

    repeat
        garageOpenAttempts += 1

        -- Fire open via ProximityPrompt if available, else fallback to remote
        local chosenPlatformObj = afFindPlatformObj(garagePos)
        local firedPrompt = false
        if chosenPlatformObj then
            for _, desc in ipairs(chosenPlatformObj:GetDescendants()) do
                if desc:IsA("ProximityPrompt") then
                    pcall(function() fireproximityprompt(desc) end)
                    firedPrompt = true
                    break
                end
            end
        end
        if not firedPrompt then
            pcall(function() PromptGarageRemote:FireServer() end)
        end

        -- Wait then check Owner attribute
        task.wait(1.2)
        if not AF.running then return end

        garageOpenConfirmed = afGarageIsOpen(garagePos)

        if garageOpenConfirmed then
            afLog("  Garage confirmed open ✓ (Owner = " .. tostring(player.Name) .. ")", ACCENT_GRN)
        else
            local plat = afFindPlatformObj(garagePos)
            local own = plat and tostring(plat:GetAttribute("Owner") or "none") or "?"
            afLog("  ⚠ Attempt " .. garageOpenAttempts .. " — Owner='" .. own .. "', retrying...", Color3.fromRGB(255, 160, 50))
            -- Re-TP to platform before next attempt
            local r = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
            if r then r.CFrame = CFrame.new(garagePos) end
            task.wait(0.8)
        end
    until garageOpenConfirmed or garageOpenAttempts >= maxGarageAttempts or not AF.running

    if not garageOpenConfirmed then
        afLog("❌ Garage failed to open after " .. maxGarageAttempts .. " attempts — aborting cycle", Color3.fromRGB(255, 80, 80))
        return
    end
    if not AF.running then return end

    -- ── STEP 9: UPGRADE CAR (respects AF_OPTS.doUpgrade and individual toggles) ──
    if AF_OPTS.doUpgrade then
        afLog("⚡ Upgrading car...", Color3.fromRGB(255, 215, 50))
        -- garagePos set by platform check above

        local function afEnsureGarage()
            if not afGarageOurs(garagePos) then
                afLog("  ⚠ Platform taken — waiting...", Color3.fromRGB(255,80,80))
                afWaitForGarage(garagePos)
            end
            local r = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
            if not r then return end
            r.CFrame = CFrame.new(garagePos)
            task.wait(0.8)
            pcall(function() PromptGarageRemote:FireServer() end)
            task.wait(1.5)
        end

        local function afBuy(label, buyFn, cooldown, maxRetries)
            cooldown = cooldown or 0; maxRetries = maxRetries or 8  -- 8 attempts: if no money change, skip (already owned)
            local mb = afGetMoney(); local att = 0
            repeat
                -- Pause if platform lost
                if not afGarageOurs(garagePos) then
                    afLog("  ⏸ Platform lost during " .. label .. " — waiting for it back", Color3.fromRGB(255,80,80))
                    if not afWaitForGarage(garagePos) then return false end
                    afEnsureGarage()
                    mb = afGetMoney()
                end
                att += 1
                afLog("  " .. label .. " (" .. att .. ")")
                pcall(buyFn)
                for _=1,8 do if not AF.running then break end task.wait(0.05) end
            until afGetMoney() < mb or att >= maxRetries or not AF.running

            if afGetMoney() < mb then
                -- confirmed bought
            elseif att >= maxRetries then
                afLog("  ⏭ " .. label .. " skipped (already owned or unavailable)", Color3.fromRGB(180,180,180))
            end
            if cooldown > 0 and AF.running then task.wait(cooldown) end
        end

        local u = AF_OPTS.upgrades
        if u.Fix      then pcall(function() FixRemote:InvokeServer() end); task.wait(0.5) end
        if u.Accel1   then afBuy("AccelLvl 1", function() Remote:InvokeServer("Install",{section="AccelerationLvl",level=1}) end) end
        if u.Accel2   then afBuy("AccelLvl 2", function() Remote:InvokeServer("Install",{section="AccelerationLvl",level=2}) end) end
        if u.Accel3   then afBuy("AccelLvl 3", function() Remote:InvokeServer("Install",{section="AccelerationLvl",level=3}) end) end
        if u.Engine1  then afBuy("EngineLvl 1",function() Remote:InvokeServer("Install",{section="EngineLvl",level=1}) end) end
        if u.Engine2  then afBuy("EngineLvl 2",function() Remote:InvokeServer("Install",{section="EngineLvl",level=2}) end) end
        if u.Nitro1   then afBuy("NitroLvl 1", function() Remote:InvokeServer("Install",{section="NitroLvl",level=1}) end) end
        if u.Nitro2   then afBuy("NitroLvl 2", function() Remote:InvokeServer("Install",{section="NitroLvl",level=2}) end) end
        if u.Nitro3   then afBuy("NitroLvl 3", function() Remote:InvokeServer("Install",{section="NitroLvl",level=3}) end) end
        if u.Paint     then afBuy("Paint",     function() Remote:InvokeServer("Install",{section="Paint",     upgradeName="Black",   requestedColors={Color1=Color3.new(0.157,0.157,0.157)}}) end) end
        if u.Light     then afBuy("LightColor",function() Remote:InvokeServer("Install",{section="LightColor",upgradeName="Black",   requestedColors={Color1=Color3.new(0.157,0.157,0.157)}}) end) end
        if u.NitroCol  then afBuy("NitroColor",function() Remote:InvokeServer("Install",{section="NitroColor",upgradeName="JetFlame"}) end, 2) end
        if u.Underglow then afBuy("Underglow", function() Remote:InvokeServer("Install",{section="Underglow", upgradeName="TwoColorFrontBackSplit",requestedColors={Secondary=Color3.new(0.82,0.54,0.25),Primary=Color3.new(0.23,0.50,0.78)}}) end, 2) end

        if not AF.running then return end
    else
        afLog("⏭ Upgrade skipped (disabled in options)")
    end

    -- ── STEP 10: CLOSE GARAGE UI (wait until confirmed closed before leaving) ──
    afLog("🚪 Closing garage...")
    pcall(function() PromptGarageRemote:FireServer() end)
    task.wait(0.8)

    -- Validator: keep firing until the garage GUI is gone (up to 5s)
    -- The garage UI is typically under PlayerGui — look for a known garage screen
    local garageCloseWait = 0
    local function isGarageOpen()
        local pg = player.PlayerGui
        -- Common garage GUI names — adjust if needed
        for _, name in ipairs({"GarageUI", "GarageMenu", "Garage", "WorkshopUI", "ShopUI"}) do
            local g = pg:FindFirstChild(name, true)
            if g and (g:IsA("ScreenGui") or g:IsA("Frame")) and g.Visible then
                return true
            end
        end
        return false
    end
    while isGarageOpen() and garageCloseWait < 5 do
        pcall(function() PromptGarageRemote:FireServer() end)
        task.wait(0.5)
        garageCloseWait += 0.5
    end
    -- Final hard wait so any transition animation finishes
    task.wait(0.8)

    -- ── STEP 11: TELEPORT TO PLOT (smartTeleport — moves car+player if seated) ──
    afLog("🏡 Going to plot...")
    local plotCF = getPlotCFrame()
    if plotCF then
        smartTeleport(plotCF)
    end
    task.wait(1)
    if not AF.running then return end

    -- ── STEP 12: EXIT CAR — must happen before going to desk ──
    afLog("🚶 Exiting car...")
    local hum = player.Character and player.Character:FindFirstChildOfClass("Humanoid")
    if hum and hum.SeatPart then
        -- Force unseat: try afExitCar first, then hard unseat
        afExitCar(carModel)
        task.wait(0.5)
        -- If still seated, force it
        if hum.SeatPart then
            pcall(function() hum.Sit = false end)
            task.wait(0.3)
        end
        -- Wait until confirmed out of seat (up to 4s)
        local exitWait = 0
        while hum.SeatPart and exitWait < 4 do
            pcall(function() hum.Sit = false end)
            task.wait(0.2)
            exitWait += 0.2
        end
        if hum.SeatPart then
            afLog("  ⚠ Still seated after 4s — proceeding anyway")
        else
            afLog("  Out of car ✓", ACCENT_GRN)
        end
    else
        afLog("  Not in car, continuing...")
    end
    task.wait(0.3)
    if not AF.running then return end

    -- ── STEP 13: TELEPORT TO DESK ──
    afLog("🖥 Going to desk...")
    local plotName = nil
    local plotVal = player:FindFirstChild("Plot")
    if plotVal then
        plotName = (plotVal:IsA("StringValue") and plotVal.Value)
            or (plotVal:IsA("ObjectValue") and plotVal.Value and plotVal.Value.Name)
    end
    local desk = plotName
        and workspace:FindFirstChild("Plots")
        and workspace.Plots:FindFirstChild(plotName)
        and workspace.Plots[plotName]:FindFirstChild("Objects", true)
        and workspace.Plots[plotName].Objects:FindFirstChild("BigDesk")

    if desk then
        local root2 = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
        if root2 then
            -- desk may be a Model — get its position safely
            local deskPos
            if desk:IsA("BasePart") then
                deskPos = desk.CFrame
            elseif desk:IsA("Model") then
                local ok, cf = pcall(function() return desk:GetPivot() end)
                if ok then
                    deskPos = cf
                else
                    local part = desk.PrimaryPart or desk:FindFirstChildWhichIsA("BasePart", true)
                    deskPos = part and part.CFrame
                end
            end
            if deskPos then
                root2.CFrame = deskPos + Vector3.new(0, 3, 0)
            else
                afLog("  ⚠ Could not read desk position")
            end
        end
        task.wait(0.5)
        -- Open PC
        local pp = desk:FindFirstChild("MainSeat", true)
        pp = pp and pp:FindFirstChildOfClass("ProximityPrompt")
        if pp then
            pcall(function() fireproximityprompt(pp) end)
            task.wait(1.5)
        end
    else
        afLog("  ⚠ Desk not found — trying direct listing")
    end
    if not AF.running then return end

    -- ── STEP 14: CALCULATE SELL PRICE ──
    local listPrice = 0
    local totalIn   = 0   -- hoisted so it's in scope for profit row at STEP 17
    if AF_OPTS.doSell then
        afLog("💰 Calculating sell price...")
        newCarFolder = player:FindFirstChild("Cars") and player.Cars:FindFirstChild(newCarFolder.Name)
        local lvl     = getPlayerLevel()
        local marg    = calcMargin(lvl)
        -- If auto-accept enabled, reduce margin by 3% so deals close faster
        if AF_OPTS.doAccept then marg = math.max(0, marg - 0.03) end
        local bought2 = tonumber((newCarFolder and newCarFolder:GetAttribute("BoughtPrice")))  or finalPrice
        local checkP  = tonumber((newCarFolder and newCarFolder:GetAttribute("CheckPrice")))   or 0
        local fixP    = tonumber((newCarFolder and newCarFolder:GetAttribute("FixPrice")))     or 0
        local spent   = tonumber((newCarFolder and newCarFolder:GetAttribute("MoneySpent")))   or 0
        totalIn   = bought2 + checkP + fixP + spent
        listPrice = math.floor(totalIn + totalIn * marg)
        afLog("  List at: $" .. formatNumber(listPrice) .. (AF_OPTS.doAccept and " (−3% for auto-accept)" or ""), Color3.fromRGB(255, 215, 50))

        -- ── STEP 15: LIST CAR FOR SALE ──
        afLog("📋 Listing car for sale...")
        carModel = afGetCarModel(newCarFolder)

        -- If car missing from PlayersCars, try recovery before giving up
        if not carModel then
            afLog("  ⚠ Car not in plot — running recovery...")
            local plotCFforRecover = getPlotCFrame()
            afRecoverCar(newCarFolder, plotCFforRecover)
            task.wait(1)
            carModel = afGetCarModel(newCarFolder)
        end

        if carModel then
            local ok, err = pcall(function()
                PromptSellRemote:InvokeServer("ListVehicle", carModel, listPrice, false)
            end)
            if ok then
                afLog("  Listed ✓", ACCENT_GRN)
                AF.saved.carModel  = carModel
                AF.saved.listPrice = listPrice
                AF.saved.desk      = desk
                AF.saved.phase     = "waiting"
                -- (profit row logged after actual NPC sale below)
            else
                afLog("  ⚠ List failed: " .. tostring(err))
            end
        else
            afLog("  ⚠ Car model missing for listing")
        end
        task.wait(0.5)
    else
        afLog("⏭ Sell skipped (disabled in options)")
    end

    -- ── STEP 16: RESET PLAYER (BreakJoints) ──
    task.wait(1)  -- wait 1s after listing before resetting
    afLog("🔄 Resetting player...")
    pcall(function() player.Character:BreakJoints() end)
    local respawnWait = 0
    repeat
        task.wait(0.3); respawnWait += 0.3
    until (player.Character and player.Character:FindFirstChild("HumanoidRootPart")
        and player.Character:FindFirstChildOfClass("Humanoid")
        and player.Character:FindFirstChildOfClass("Humanoid").Health > 0)
        or respawnWait >= 8
    task.wait(3)  -- wait 3s after respawn before teleporting so character is fully loaded

    afLog("  Respawned ✓ — teleporting to plot", ACCENT_GRN)
    local plotCF2   = getPlotCFrame()
    local rootFinal = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
    if plotCF2 and rootFinal then
        rootFinal.CFrame = plotCF2
        task.wait(0.8)
        -- Validate TP landed: check player is within 20 studs of plot, retry up to 3x
        for _=1,3 do
            rootFinal = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
            if rootFinal and (rootFinal.Position - plotCF2.Position).Magnitude < 20 then break end
            afLog("  ⚠ TP didn't land — retrying...", Color3.fromRGB(255,160,50))
            rootFinal = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
            if rootFinal then rootFinal.CFrame = plotCF2 end
            task.wait(0.8)
        end
    else
        task.wait(0.8)
    end

    -- ── STEP 17: WAIT FOR NPC BUYERS AND AUTO-ACCEPT (if enabled) ──
    if AF_OPTS.doSell and AF_OPTS.doAccept and listPrice > 0 and carModel then
        afLog("👥 Watching for NPC buyers near plot...", Color3.fromRGB(100, 200, 255))
        local plotPos = plotCF2 and plotCF2.Position
        local PLOT_RADIUS = 350

        local buyerWait = 0
        local maxBuyerWait = 120
        local sold = false

        while buyerWait < maxBuyerWait and AF.running and not sold do
            -- Every 60s re-TP player to plot in case they drifted
            if buyerWait > 0 and buyerWait % 60 == 0 then
                afLog("  📍 Re-teleporting to plot while waiting...", TEXT_GRAY)
                plotCF2 = getPlotCFrame() or plotCF2
                local rr = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
                if rr and plotCF2 then rr.CFrame = plotCF2 end
                plotPos = plotCF2 and plotCF2.Position
            end
            local charsFolder = workspace:FindFirstChild("Characters")
            if charsFolder and plotPos then
                for _, npc in ipairs(charsFolder:GetChildren()) do
                    local npcRoot = npc:FindFirstChild("HumanoidRootPart")
                    if npcRoot and (npcRoot.Position - plotPos).Magnitude < PLOT_RADIUS then
                        local botPrompt = npc:FindFirstChild("BotInteractionPrompt")
                        if botPrompt and botPrompt:IsA("ProximityPrompt") then
                            afLog("🤝 Buyer found: " .. npc.Name, ACCENT_GRN)
                            pcall(function() fireproximityprompt(botPrompt) end)
                            task.wait(0.5)
                            local carUUID = newCarFolder and newCarFolder.Name or ""
                            local cModelInWorld = workspace:FindFirstChild("PlayersCars")
                                and workspace.PlayersCars:FindFirstChild(carUUID)
                            cModelInWorld = cModelInWorld or carModel

                            -- Snapshot money BEFORE confirming sale
                            local moneyBefore = afGetMoney()

                            local ok2, res = pcall(function()
                                return PromptSellRemote:InvokeServer(
                                    "RespondToBot", npc, cModelInWorld, listPrice, "ConfirmSell"
                                )
                            end)
                            if ok2 and res then
                                -- Wait for server to update money (retry up to 3s)
                                local moneyAfter = moneyBefore
                                for _=1,6 do
                                    task.wait(0.5)
                                    local m = afGetMoney()
                                    if m ~= moneyBefore then moneyAfter = m; break end
                                end
                                do
                                    local gained = moneyAfter - moneyBefore
                                    local saleAmt = gained > 1000 and gained or listPrice
                                    afLog("✅ SOLD to "..npc.Name.." — $"..formatNumber(saleAmt), Color3.fromRGB(80,255,140))
                                    addProfitRow(newCarFolder and newCarFolder.Name or "Unknown", totalIn, saleAmt)
                                end
                                sold = true
                                break
                            else
                                afLog("  Offer check: " .. tostring(res), TEXT_GRAY)
                            end
                        end
                    end
                end
            end
            if not sold then
                task.wait(2)
                buyerWait += 2
                if buyerWait % 20 == 0 then
                    afLog("  Still waiting for buyer... (" .. buyerWait .. "s)", TEXT_GRAY)
                end
            end
        end

        if not sold then
            afLog("⚠ No buyer confirmed in " .. maxBuyerWait .. "s — moving on", Color3.fromRGB(255,180,0))
        end
    end

    -- Clear saved state — cycle fully done
    AF.saved.active = false
    farmResumeBar.Visible = false
    afLog("🏁 Cycle complete!", Color3.fromRGB(255, 215, 50))
end

-- A partial cycle runner that skips to the saved phase
local function afRunResume()
    local s = AF.saved
    if not s.active or not s.newCarFolder then
        afLog("⚠ No saved state to resume")
        return
    end
    afLog("↩ Resuming from phase: " .. s.phase, Color3.fromRGB(255, 200, 50))

    local newCarFolder = s.newCarFolder
    local carModel     = s.carModel or afGetCarModel(newCarFolder)
    local listPrice    = s.listPrice
    local desk         = s.desk

    if s.phase == "upgrade" then
        -- Re-run from garage open
        local garagePos = Vector3.new(-284, 8, 4465)
        local root = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
        if root then root.CFrame = CFrame.new(garagePos) end
        task.wait(1)
        pcall(function() PromptGarageRemote:FireServer() end)
        task.wait(1.5)
        -- (upgrades will be handled inline below — just jump afRunCycle from step 9)
        -- Re-invoke afRunCycle but it won't re-buy the car since we skip scan
        -- Instead we duplicate the upgrade+listing block here
        afLog("⚡ Resuming upgrades...", Color3.fromRGB(255, 215, 50))
        local function afBuyR(label, buyFn, cooldown, maxRetries)
            cooldown = cooldown or 0; maxRetries = maxRetries or 25
            local mb = afGetMoney(); local att = 0
            repeat att+=1; afLog("  "..label.." ("..att..")"); pcall(buyFn); for _=1,8 do if not AF.running then break end task.wait(0.05) end
            until afGetMoney()<mb or att>=maxRetries or not AF.running
            if afGetMoney()>=mb and att>=maxRetries and AF.running then
                afLog("  ⚠ "..label.." — re-opening garage...")
                local r2=player.Character and player.Character:FindFirstChild("HumanoidRootPart")
                if r2 then r2.CFrame=CFrame.new(garagePos) end; task.wait(0.8)
                pcall(function() PromptGarageRemote:FireServer() end); task.wait(1.5)
                local mb2=afGetMoney(); local att2=0
                repeat att2+=1; pcall(buyFn); task.wait(0.4) until afGetMoney()<mb2 or att2>=15 or not AF.running
            end
            if cooldown>0 and AF.running then task.wait(cooldown) end
        end
        pcall(function() FixRemote:InvokeServer() end); task.wait(0.5)
        for i=1,3 do afBuyR("AccelLvl "..i,function() Remote:InvokeServer("Install",{section="AccelerationLvl",level=i}) end) end
        for i=1,2 do afBuyR("EngineLvl "..i,function() Remote:InvokeServer("Install",{section="EngineLvl",level=i}) end) end
        for i=1,3 do afBuyR("NitroLvl "..i,function() Remote:InvokeServer("Install",{section="NitroLvl",level=i}) end) end
        afBuyR("Paint",      function() Remote:InvokeServer("Install",{section="Paint",     upgradeName="Black",    requestedColors={Color1=Color3.new(0.157,0.157,0.157)}}) end)
        afBuyR("LightColor", function() Remote:InvokeServer("Install",{section="LightColor",upgradeName="Black",    requestedColors={Color1=Color3.new(0.157,0.157,0.157)}}) end)
        afBuyR("NitroColor", function() Remote:InvokeServer("Install",{section="NitroColor",upgradeName="JetFlame"}) end, 2)
        afBuyR("Underglow",  function() Remote:InvokeServer("Install",{section="Underglow", upgradeName="TwoColorFrontBackSplit",requestedColors={Secondary=Color3.new(0.82,0.54,0.25),Primary=Color3.new(0.23,0.50,0.78)}}) end, 2)
        if not AF.running then return end
        pcall(function() PromptGarageRemote:FireServer() end); task.wait(1)
        s.phase = "listing"
    end

    if s.phase == "listing" then
        -- Go to plot and list
        local rootP = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
        local plotCF = getPlotCFrame()
        if plotCF and rootP then rootP.CFrame = plotCF end
        task.wait(1)
        -- Find desk
        local plotVal = player:FindFirstChild("Plot")
        local plotName = plotVal and ((plotVal:IsA("StringValue") and plotVal.Value)
            or (plotVal:IsA("ObjectValue") and plotVal.Value and plotVal.Value.Name))
        desk = plotName
            and workspace:FindFirstChild("Plots")
            and workspace.Plots:FindFirstChild(plotName)
            and workspace.Plots[plotName]:FindFirstChild("Objects",true)
            and workspace.Plots[plotName].Objects:FindFirstChild("BigDesk")
        if desk then
            local deskPos
            local ok2,cf2=pcall(function() return desk:GetPivot() end)
            if ok2 then deskPos=cf2 elseif desk:IsA("BasePart") then deskPos=desk.CFrame end
            if deskPos and rootP then rootP.CFrame = deskPos + Vector3.new(0,3,0) end
            task.wait(0.5)
            local pp=desk:FindFirstChild("MainSeat",true)
            pp=pp and pp:FindFirstChildOfClass("ProximityPrompt")
            if pp then pcall(function() fireproximityprompt(pp) end); task.wait(1.5) end
        end
        -- Calculate price
        newCarFolder = player:FindFirstChild("Cars") and player.Cars:FindFirstChild(newCarFolder.Name)
        local lvl=getPlayerLevel(); local marg=calcMargin(lvl)
        local b=tonumber(newCarFolder and newCarFolder:GetAttribute("BoughtPrice")) or 0
        local c=tonumber(newCarFolder and newCarFolder:GetAttribute("CheckPrice")) or 0
        local f=tonumber(newCarFolder and newCarFolder:GetAttribute("FixPrice")) or 0
        local sp=tonumber(newCarFolder and newCarFolder:GetAttribute("MoneySpent")) or 0
        local tot=b+c+f+sp; listPrice=math.floor(tot+tot*marg)
        carModel=afGetCarModel(newCarFolder)
        if carModel then
            pcall(function() PromptSellRemote:InvokeServer("ListVehicle",carModel,listPrice,false) end)
            afLog("  Listed at $"..formatNumber(listPrice).." ✓", ACCENT_GRN)
            AF.saved.carModel=carModel; AF.saved.listPrice=listPrice; AF.saved.desk=desk
        end
        afClosePC(desk)
        s.phase = "waiting"
    end

    if s.phase == "waiting" then
        -- Wait for buyer
        carModel = s.carModel or afGetCarModel(newCarFolder)
        listPrice = s.listPrice
        afLog("⏳ Waiting for buyer (resumed)...")
        local carIdClean = newCarFolder.Name:sub(2,-2)
        local npcModel2 = nil
        afWait(function()
            local chars=workspace:FindFirstChild("Characters"); if not chars then return false end
            for _,c in ipairs(chars:GetChildren()) do
                if c.Name:find(carIdClean,1,true) then npcModel2=c; return true end
            end; return false
        end, 90)
        if npcModel2 then
            afLog("🧑 Buyer found!", ACCENT_GRN)
            for _,d in ipairs(npcModel2:GetDescendants()) do
                if d:IsA("ProximityPrompt") then pcall(function() fireproximityprompt(d) end); break end
            end
            local waited2=0
            while waited2<120 and AF.running do
                if carModel then
                    local ok3,res=pcall(function()
                        return PromptSellRemote:InvokeServer("RespondToBot",npcModel2,carModel,listPrice,"ConfirmSell")
                    end)
                    if ok3 and res then afLog("✅ SOLD at $"..formatNumber(listPrice),ACCENT_GRN); break end
                end
                task.wait(2); waited2+=2
            end
        else
            afLog("⚠ No buyer in 90s")
        end
    end

    AF.saved.active = false
    farmResumeBar.Visible = false
    afLog("🏁 Resume complete!", Color3.fromRGB(255,215,50))
end  -- close afRunResume

farmResumeBtn.MouseButton1Click:Connect(function()
    if AF.running then return end
    AF.running = true
    farmStartBtn.Active = false
    farmStartBtn.Text = "⏳ RUNNING..."
    farmStartGrad.Enabled = false
    farmStartBtn.BackgroundColor3 = Color3.fromRGB(40,40,55)
    task.spawn(function()
        local ok, err = pcall(afRunResume)
        if not ok then afLog("💥 Resume error: "..tostring(err), Color3.fromRGB(255,80,80)) end
        AF.running = false
        farmStartBtn.Text = "▶ START FARM"
        farmStartGrad.Enabled = true
        farmStartBtn.BackgroundColor3 = ACCENT_GRN
        farmStartBtn.Active = true
    end)
end)

-- ===========================
-- LOGIC: PROCESS MY CURRENT CAR
-- Skips scan/buy — takes the car you're driving right now and processes it
-- ===========================
local function afRunMyCar()
    afLog("🚗 Processing current car...", Color3.fromRGB(100, 180, 255))

    -- 1. Detect which car model the player is currently in
    local hum0 = player.Character and player.Character:FindFirstChildOfClass("Humanoid")
    local seatPart = hum0 and hum0.SeatPart
    local carModel = seatPart and seatPart:FindFirstAncestorOfClass("Model")
    if not carModel then
        -- Not seated — check if they're standing near a car in PlayersCars
        afLog("  Not seated. Looking for nearby car in PlayersCars...")
        local pcarsF = workspace:FindFirstChild("PlayersCars")
        local root0  = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
        if pcarsF and root0 then
            local closest, closestDist = nil, 20
            for _, m in ipairs(pcarsF:GetChildren()) do
                local ok, pivot = pcall(function() return m:GetPivot() end)
                if ok then
                    local d = (root0.Position - pivot.Position).Magnitude
                    if d < closestDist then closest = m; closestDist = d end
                end
            end
            carModel = closest
        end
    end

    if not carModel then
        afLog("❌ No car found — get in your car first!", Color3.fromRGB(255,80,80))
        return
    end
    afLog("  Car: " .. carModel.Name, ACCENT_GRN)

    -- 2. Find which folder in player.Cars matches this model
    local newCarFolder = nil
    local carsF = player:FindFirstChild("Cars")
    if carsF then
        for _, cf in ipairs(carsF:GetChildren()) do
            if cf.Name == carModel.Name then
                newCarFolder = cf
                break
            end
        end
    end
    if not newCarFolder then
        afLog("⚠ Car folder not found in player.Cars — listing may use zero cost", Color3.fromRGB(255,180,0))
    end

    -- Save state for resume
    AF.saved.active       = true
    AF.saved.newCarFolder = newCarFolder
    AF.saved.phase        = "upgrade"
    farmResumeBar.Visible = true

    -- Check if we should skip garage: upgrade disabled OR car already has 700k+ spent
    local moneySpentOnCar = tonumber(newCarFolder and newCarFolder:GetAttribute("MoneySpent")) or 0
    local skipGarage = (not AF_OPTS.doUpgrade) or (moneySpentOnCar >= 700000)

    if not AF_OPTS.doUpgrade then
        afLog("⏭ Garage skipped (Upgrade OFF in options)", Color3.fromRGB(180, 180, 180))
    elseif skipGarage then
        afLog("💰 MoneySpent $" .. formatNumber(moneySpentOnCar) .. " ≥ $700k — skipping garage", Color3.fromRGB(255, 200, 80))
    end

    if not skipGarage then
    -- 3. Check / pick garage platform (use shared GARAGE_PLATFORMS)
    afLog("🏠 Checking garage platforms...")
    local garagePlatformsFolder2 = workspace:FindFirstChild("Garage_Contents")
        and workspace.Garage_Contents:FindFirstChild("GaragePlatforms")
    local chosenPlatform = nil
    if garagePlatformsFolder2 then
        for _, plat in ipairs(garagePlatformsFolder2:GetChildren()) do
            local platPos = plat:IsA("BasePart") and plat.Position
                or (plat:IsA("Model") and plat.PrimaryPart and plat.PrimaryPart.Position)
            if platPos then
                for _, pd in ipairs(GARAGE_PLATFORMS) do
                    if (platPos - pd.cf.Position).Magnitude < 6 then
                        local owner = plat:GetAttribute("Owner")
                        local isFree = (not owner) or (owner=="") or tostring(owner)==tostring(player.UserId) or tostring(owner)==player.Name
                        if isFree and not chosenPlatform then chosenPlatform=pd; afLog("  "..pd.label.." is free ✓",ACCENT_GRN) end
                        break
                    end
                end
            end
        end
    end
    if not chosenPlatform then chosenPlatform=GARAGE_PLATFORMS[1]; afLog("  ⚠ Defaulting to Platform A") end
    local garagePos = chosenPlatform.pos

    -- 4. Teleport car + player to garage
    afLog("🏠 Teleporting to " .. chosenPlatform.label .. "...")
    smartTeleport(CFrame.new(garagePos))
    task.wait(1)
    if not AF.running then return end

    -- 5. Open garage — retry until Owner attribute confirms it's open
    afLog("🔧 Opening garage...")
    local garageOpen2Confirmed = false
    local garageOpen2Attempts  = 0

    repeat
        garageOpen2Attempts += 1
        local platObj2 = afFindPlatformObj(garagePos)
        local firedPrompt2 = false
        if platObj2 then
            for _, desc in ipairs(platObj2:GetDescendants()) do
                if desc:IsA("ProximityPrompt") then
                    pcall(function() fireproximityprompt(desc) end)
                    firedPrompt2 = true
                    break
                end
            end
        end
        if not firedPrompt2 then
            pcall(function() PromptGarageRemote:FireServer() end)
        end
        task.wait(1.2)
        if not AF.running then return end

        garageOpen2Confirmed = afGarageIsOpen(garagePos)
        if garageOpen2Confirmed then
            afLog("  Garage confirmed open ✓", ACCENT_GRN)
        else
            local plat2 = afFindPlatformObj(garagePos)
            local own2 = plat2 and tostring(plat2:GetAttribute("Owner") or "none") or "?"
            afLog("  ⚠ Attempt " .. garageOpen2Attempts .. " — Owner='" .. own2 .. "', retrying...", Color3.fromRGB(255, 160, 50))
            local r2 = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
            if r2 then r2.CFrame = CFrame.new(garagePos) end
            task.wait(0.8)
        end
    until garageOpen2Confirmed or garageOpen2Attempts >= 8 or not AF.running

    if not garageOpen2Confirmed then
        afLog("❌ Garage failed to open — aborting", Color3.fromRGB(255, 80, 80))
        return
    end
    if not AF.running then return end

    -- 6. Upgrade (respects AF_OPTS)
    if AF_OPTS.doUpgrade then
        afLog("⚡ Upgrading car...", Color3.fromRGB(255,215,50))

        local function afEnsureGarage2()
            if not afGarageOurs(garagePos) then
                afLog("  ⏸ Platform taken — waiting...", Color3.fromRGB(255,80,80))
                afWaitForGarage(garagePos)
            end
            local r = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
            if r then r.CFrame = CFrame.new(garagePos) end
            task.wait(0.8); pcall(function() PromptGarageRemote:FireServer() end); task.wait(1.5)
        end

        local function afBuy2(label, buyFn, cooldown, maxRetries)
            cooldown=cooldown or 0; maxRetries=maxRetries or 8
            local mb=afGetMoney(); local att=0
            repeat
                if not afGarageOurs(garagePos) then
                    afLog("  ⏸ Platform lost during "..label,Color3.fromRGB(255,80,80))
                    if not afWaitForGarage(garagePos) then return end
                    afEnsureGarage2(); mb=afGetMoney()
                end
                att+=1; afLog("  "..label.." ("..att..")"); pcall(buyFn); for _=1,8 do if not AF.running then break end task.wait(0.05) end
            until afGetMoney()<mb or att>=maxRetries or not AF.running
            if att>=maxRetries and afGetMoney()>=mb then
                afLog("  ⏭ "..label.." skipped (already owned or unavailable)",Color3.fromRGB(180,180,180))
            end
            if cooldown>0 and AF.running then task.wait(cooldown) end
        end

        local u = AF_OPTS.upgrades
        if u.Fix      then pcall(function() FixRemote:InvokeServer() end); task.wait(0.5) end
        if u.Accel1   then afBuy2("AccelLvl 1",  function() Remote:InvokeServer("Install",{section="AccelerationLvl",level=1}) end) end
        if u.Accel2   then afBuy2("AccelLvl 2",  function() Remote:InvokeServer("Install",{section="AccelerationLvl",level=2}) end) end
        if u.Accel3   then afBuy2("AccelLvl 3",  function() Remote:InvokeServer("Install",{section="AccelerationLvl",level=3}) end) end
        if u.Engine1  then afBuy2("EngineLvl 1", function() Remote:InvokeServer("Install",{section="EngineLvl",level=1}) end) end
        if u.Engine2  then afBuy2("EngineLvl 2", function() Remote:InvokeServer("Install",{section="EngineLvl",level=2}) end) end
        if u.Nitro1   then afBuy2("NitroLvl 1",  function() Remote:InvokeServer("Install",{section="NitroLvl",level=1}) end) end
        if u.Nitro2   then afBuy2("NitroLvl 2",  function() Remote:InvokeServer("Install",{section="NitroLvl",level=2}) end) end
        if u.Nitro3   then afBuy2("NitroLvl 3",  function() Remote:InvokeServer("Install",{section="NitroLvl",level=3}) end) end
        if u.Paint     then afBuy2("Paint",      function() Remote:InvokeServer("Install",{section="Paint",     upgradeName="Black",   requestedColors={Color1=Color3.new(0.157,0.157,0.157)}}) end) end
        if u.Light     then afBuy2("LightColor", function() Remote:InvokeServer("Install",{section="LightColor",upgradeName="Black",   requestedColors={Color1=Color3.new(0.157,0.157,0.157)}}) end) end
        if u.NitroCol  then afBuy2("NitroColor", function() Remote:InvokeServer("Install",{section="NitroColor",upgradeName="JetFlame"}) end, 2) end
        if u.Underglow then afBuy2("Underglow",  function() Remote:InvokeServer("Install",{section="Underglow", upgradeName="TwoColorFrontBackSplit",requestedColors={Secondary=Color3.new(0.82,0.54,0.25),Primary=Color3.new(0.23,0.50,0.78)}}) end, 2) end
        if not AF.running then return end
    else
        afLog("⏭ Upgrade skipped (disabled)")
    end

    -- Close garage
    afLog("🚪 Closing garage...")
    pcall(function() PromptGarageRemote:FireServer() end)
    task.wait(1.2)

    end -- end if not skipGarage

    -- Teleport to plot
    afLog("🏡 Going to plot...")
    local plotCF = getPlotCFrame()
    if plotCF then smartTeleport(plotCF) end
    task.wait(1)
    if not AF.running then return end

    -- Exit car
    afLog("🚶 Exiting car...")
    local hum2 = player.Character and player.Character:FindFirstChildOfClass("Humanoid")
    if hum2 and hum2.SeatPart then
        afExitCar(carModel)
        local ew=0
        while hum2.SeatPart and ew<4 do pcall(function() hum2.Sit=false end); task.wait(0.2); ew+=0.2 end
        if not hum2.SeatPart then afLog("  Out of car ✓", ACCENT_GRN) end
    end
    task.wait(0.3)
    if not AF.running then return end

    local listPrice2 = 0
    local plotCF3 = getPlotCFrame()

    if AF_OPTS.doSell then
        -- Go to desk
        afLog("🖥 Going to desk...")
        local plotVal2 = player:FindFirstChild("Plot")
        local plotName2 = plotVal2 and ((plotVal2:IsA("StringValue") and plotVal2.Value)
            or (plotVal2:IsA("ObjectValue") and plotVal2.Value and plotVal2.Value.Name))
        local desk2 = plotName2
            and workspace:FindFirstChild("Plots")
            and workspace.Plots:FindFirstChild(plotName2)
            and workspace.Plots[plotName2]:FindFirstChild("Objects",true)
            and workspace.Plots[plotName2].Objects:FindFirstChild("BigDesk")
        if desk2 then
            local r2 = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
            if r2 then
                local deskPos; local ok2,cf2 = pcall(function() return desk2:GetPivot() end)
                if ok2 then deskPos=cf2 elseif desk2:IsA("BasePart") then deskPos=desk2.CFrame end
                if deskPos then r2.CFrame = deskPos + Vector3.new(0,3,0) end
            end
            task.wait(0.5)
            local pp2 = desk2:FindFirstChild("MainSeat",true)
            pp2 = pp2 and pp2:FindFirstChildOfClass("ProximityPrompt")
            if pp2 then pcall(function() fireproximityprompt(pp2) end); task.wait(1.5) end
        end
        if not AF.running then return end

        -- Calculate price
        afLog("💰 Calculating sell price...")
        newCarFolder = newCarFolder and player:FindFirstChild("Cars") and player.Cars:FindFirstChild(newCarFolder.Name)
        local lvl2 = getPlayerLevel(); local marg2 = calcMargin(lvl2)
        if AF_OPTS.doAccept then marg2 = math.max(0, marg2 - 0.03) end
        local b2=tonumber(newCarFolder and newCarFolder:GetAttribute("BoughtPrice")) or 0
        local c2=tonumber(newCarFolder and newCarFolder:GetAttribute("CheckPrice"))  or 0
        local f2=tonumber(newCarFolder and newCarFolder:GetAttribute("FixPrice"))    or 0
        local sp2=tonumber(newCarFolder and newCarFolder:GetAttribute("MoneySpent")) or 0
        listPrice2 = math.floor((b2+c2+f2+sp2)*(1+marg2))
        afLog("  List at: $"..formatNumber(listPrice2)..(AF_OPTS.doAccept and " (−3% auto-accept)" or ""), Color3.fromRGB(255,215,50))

        -- List car
        afLog("📋 Listing car for sale...")
        carModel = afGetCarModel(newCarFolder) or carModel
        if not carModel then
            afLog("  ⚠ Car not in plot — running recovery...")
            afRecoverCar(newCarFolder, plotCF3)
            task.wait(1)
            carModel = afGetCarModel(newCarFolder)
        end
        if carModel then
            local ok3,err3 = pcall(function() PromptSellRemote:InvokeServer("ListVehicle",carModel,listPrice2,false) end)
            if ok3 then
                afLog("  Listed ✓",ACCENT_GRN)
                -- (profit row logged after actual NPC sale below)
            else afLog("  ⚠ List failed: "..tostring(err3)) end
        end
        task.wait(1)

        -- Reset + TP to plot
        afLog("🔄 Resetting player...")
        pcall(function() player.Character:BreakJoints() end)
        local rw=0
        repeat task.wait(0.3); rw+=0.3
        until (player.Character and player.Character:FindFirstChild("HumanoidRootPart")
            and player.Character:FindFirstChildOfClass("Humanoid")
            and player.Character:FindFirstChildOfClass("Humanoid").Health>0) or rw>=8
        task.wait(3)

        local rootFin = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
        plotCF3 = getPlotCFrame()
        if rootFin and plotCF3 then rootFin.CFrame = plotCF3 end
        task.wait(0.8)

        -- NPC buyer wait (only if doAccept enabled)
        if AF_OPTS.doAccept and listPrice2 > 0 and carModel then
            afLog("👥 Watching for buyers near plot...", Color3.fromRGB(100,200,255))
            local plotPos2 = plotCF3 and plotCF3.Position
            local bw=0; local sold2=false
            -- Compute cost for profit row
            local b2t = tonumber(newCarFolder and newCarFolder:GetAttribute("BoughtPrice")) or 0
            local c2t = tonumber(newCarFolder and newCarFolder:GetAttribute("CheckPrice"))  or 0
            local f2t = tonumber(newCarFolder and newCarFolder:GetAttribute("FixPrice"))    or 0
            local s2t = tonumber(newCarFolder and newCarFolder:GetAttribute("MoneySpent"))  or 0
            local costIn2 = b2t+c2t+f2t+s2t

            while bw<120 and AF.running and not sold2 do
                local cf2 = workspace:FindFirstChild("Characters")
                if cf2 and plotPos2 then
                    for _, npc in ipairs(cf2:GetChildren()) do
                        local nr = npc:FindFirstChild("HumanoidRootPart")
                        if nr and (nr.Position-plotPos2).Magnitude < 350 then
                            local bp = npc:FindFirstChild("BotInteractionPrompt")
                            if bp and bp:IsA("ProximityPrompt") then
                                afLog("🤝 Buyer: "..npc.Name, ACCENT_GRN)
                                pcall(function() fireproximityprompt(bp) end); task.wait(0.5)
                                local carUUID2 = newCarFolder and newCarFolder.Name or ""
                                local cMiW = workspace:FindFirstChild("PlayersCars") and workspace.PlayersCars:FindFirstChild(carUUID2)
                                cMiW = cMiW or carModel
                                local moneyBefore2 = afGetMoney()
                                local ok4,res4 = pcall(function() return PromptSellRemote:InvokeServer("RespondToBot",npc,cMiW,listPrice2,"ConfirmSell") end)
                                if ok4 and res4 then
                                    -- Wait for server money update (retry up to 3s)
                                    local moneyAfter2 = moneyBefore2
                                    for _=1,6 do
                                        task.wait(0.5)
                                        local m2 = afGetMoney()
                                        if m2 ~= moneyBefore2 then moneyAfter2 = m2; break end
                                    end
                                    local gained2 = moneyAfter2 - moneyBefore2
                                    local actualSale2 = gained2 > 1000 and gained2 or listPrice2
                                    afLog("✅ SOLD at $"..formatNumber(actualSale2),Color3.fromRGB(80,255,140))
                                    addProfitRow(newCarFolder and newCarFolder.Name or "Unknown", costIn2, actualSale2)
                                    sold2=true; break
                                end
                            end
                        end
                    end
                end
                if not sold2 then task.wait(2); bw+=2 end
            end
            if not sold2 then afLog("⚠ No buyer in 120s",Color3.fromRGB(255,180,0)) end
        else
            afLog("⏭ Auto-accept skipped (disabled in options) — car listed, stopping here", Color3.fromRGB(180,180,180))
        end
    else
        afLog("⏭ Sell skipped (disabled in options) — stopping after garage", Color3.fromRGB(180,180,180))
    end

    AF.saved.active = false
    farmResumeBar.Visible = false
    afLog("🏁 My car cycle complete!", Color3.fromRGB(255,215,50))
end

farmMyCarBtn.MouseButton1Click:Connect(function()
    if AF.running then return end
    AF.running = true
    farmMyCarBtn.Active           = false
    farmMyCarBtn.Text             = "⏳ RUNNING..."
    farmMyCarGrad.Enabled         = false
    farmMyCarBtn.BackgroundColor3 = Color3.fromRGB(40,40,55)
    farmStartBtn.Active           = false

    task.spawn(function()
        local ok, err = pcall(afRunMyCar)
        if not ok then afLog("💥 Error: "..tostring(err), Color3.fromRGB(255,80,80)) end
        AF.running                    = false
        farmMyCarBtn.Text             = "🚗 PROCESS MY CURRENT CAR"
        farmMyCarGrad.Enabled         = true
        farmMyCarBtn.BackgroundColor3 = Color3.fromRGB(0,80,160)
        farmMyCarBtn.Active           = true
        farmStartBtn.Active           = true
    end)
end)

-- Autoloop toggle
local farmAutoloop = false
farmLoopBtn.MouseButton1Click:Connect(function()
    farmAutoloop = not farmAutoloop
    if farmAutoloop then
        farmLoopBtn.Text             = "🔁 ON"
        farmLoopBtn.BackgroundColor3 = Color3.fromRGB(0, 130, 0)
        farmLoopBtn.TextColor3       = TEXT_WHITE
    else
        farmLoopBtn.Text             = "🔁 OFF"
        farmLoopBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 65)
        farmLoopBtn.TextColor3       = TEXT_GRAY
    end
end)

local function stopAllFarming()
    AF.running = false
    modRunning = false
    -- Reset all start buttons
    farmStartBtn.Text             = "▶ START FARM"
    farmStartGrad.Enabled         = true
    farmStartBtn.BackgroundColor3 = ACCENT_GRN
    farmStartBtn.Active           = true
    farmMyCarBtn.Text             = "🚗 PROCESS MY CURRENT CAR"
    farmMyCarGrad.Enabled         = true
    farmMyCarBtn.BackgroundColor3 = Color3.fromRGB(0, 80, 160)
    farmMyCarBtn.Active           = true
end

farmStopBtn.MouseButton1Click:Connect(function()
    stopAllFarming()
    afLog("⏹ Stopped by user.", Color3.fromRGB(255, 80, 80))
end)

local guiClosed = false

closeBtn.MouseButton1Click:Connect(function()
    stopAllFarming()
    guiClosed = true
    mainFrame.Visible = false
    glowShell.Visible = false
end)

farmStartBtn.MouseButton1Click:Connect(function()
    if AF.running then return end
    AF.running = true
    farmStartBtn.Text             = "⏳ RUNNING..."
    farmStartGrad.Enabled         = false
    farmStartBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 55)
    farmStartBtn.Active           = false

    task.spawn(function()
        repeat
            if not AF.running then break end
            local ok, err = pcall(afRunCycle)
            if not ok then
                afLog("💥 Error: " .. tostring(err), Color3.fromRGB(255, 80, 80))
            end
            -- Only loop if still running AND autoloop is on — wait for full cycle to end first
            if AF.running and farmAutoloop then
                afLog("🔄 Cycle done — looping in 9s...", TEXT_GRAY)
                task.wait(9)
            end
        until not AF.running or not farmAutoloop

        AF.running = false
        farmStartBtn.Text             = "▶ START FARM"
        farmStartGrad.Enabled         = true
        farmStartBtn.BackgroundColor3 = ACCENT_GRN
        farmStartBtn.Active           = true
        afLog("⏹ Farm stopped.", TEXT_GRAY)
    end)
end)

-- ===========================
-- LOGIC: DRAG & HOTKEY
-- ===========================
local dragging, dragStart, startPos, startGlowPos
header.InputBegan:Connect(function(i)
    if i.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging     = true
        dragStart    = i.Position
        startPos     = mainFrame.Position
        startGlowPos = glowShell.Position
    end
end)
UserInputService.InputChanged:Connect(function(i)
    if dragging and i.UserInputType == Enum.UserInputType.MouseMovement then
        local d = i.Position - dragStart
        mainFrame.Position = UDim2.new(
            startPos.X.Scale, startPos.X.Offset + d.X,
            startPos.Y.Scale, startPos.Y.Offset + d.Y
        )
        glowShell.Position = UDim2.new(
            startGlowPos.X.Scale, startGlowPos.X.Offset + d.X,
            startGlowPos.Y.Scale, startGlowPos.Y.Offset + d.Y
        )
    end
end)
header.InputEnded:Connect(function(i)
    if i.UserInputType == Enum.UserInputType.MouseButton1 then dragging = false end
end)

UserInputService.InputBegan:Connect(function(input, gpe)
    if not gpe and input.KeyCode == Enum.KeyCode.Q then
        if guiClosed then return end  -- closed via X, Q does nothing
        local v = not mainFrame.Visible
        mainFrame.Visible = v
        glowShell.Visible = v
    end
end)
