-- Vape风格移动端面板 - 脚本工具中心
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- 创建ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "VapeGUI"
screenGui.ResetOnSpawn = false
screenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
screenGui.Parent = playerGui

-- 彩虹颜色函数
local function getRainbowColor(offset)
    offset = offset or 0
    local hue = (tick() * 0.5 + offset) % 1
    return Color3.fromHSV(hue, 0.8, 1)
end

-- 主开关按钮 (可拖动)
local toggleButton = Instance.new("ImageButton")
toggleButton.Name = "ToggleButton"
toggleButton.Size = UDim2.new(0, 45, 0, 45)
toggleButton.Position = UDim2.new(0, 10, 0.5, -22)
toggleButton.BackgroundTransparency = 1
toggleButton.Image = "rbxassetid://131464486691225"
toggleButton.Active = true
toggleButton.Draggable = true
toggleButton.Parent = screenGui

-- 主框架
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 180, 0, 240)
mainFrame.Position = UDim2.new(0.5, -90, 0.5, -120)
mainFrame.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
mainFrame.BorderSizePixel = 0
mainFrame.Active = true
mainFrame.Draggable = true
mainFrame.Visible = false
mainFrame.Parent = screenGui

-- 圆角
local mainCorner = Instance.new("UICorner")
mainCorner.CornerRadius = UDim.new(0, 8)
mainCorner.Parent = mainFrame

-- 边框
local border = Instance.new("UIStroke")
border.Color = Color3.fromRGB(40, 40, 40)
border.Thickness = 1
border.Parent = mainFrame

-- 顶部栏
local topBar = Instance.new("Frame")
topBar.Name = "TopBar"
topBar.Size = UDim2.new(1, 0, 0, 32)
topBar.BackgroundColor3 = Color3.fromRGB(18, 18, 18)
topBar.BorderSizePixel = 0
topBar.Parent = mainFrame

local topCorner = Instance.new("UICorner")
topCorner.CornerRadius = UDim.new(0, 8)
topCorner.Parent = topBar

-- 修复顶部圆角
local topFix = Instance.new("Frame")
topFix.Size = UDim2.new(1, 0, 0, 8)
topFix.Position = UDim2.new(0, 0, 1, -8)
topFix.BackgroundColor3 = Color3.fromRGB(18, 18, 18)
topFix.BorderSizePixel = 0
topFix.Parent = topBar

-- 标题
local title = Instance.new("TextLabel")
title.Name = "Title"
title.Size = UDim2.new(1, -40, 1, 0)
title.Position = UDim2.new(0, 10, 0, 0)
title.BackgroundTransparency = 1
title.Text = "工具中心"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.TextSize = 13
title.Font = Enum.Font.GothamBold
title.TextXAlignment = Enum.TextXAlignment.Left
title.Parent = topBar

-- 最小化按钮
local minimizeButton = Instance.new("TextButton")
minimizeButton.Name = "MinimizeButton"
minimizeButton.Size = UDim2.new(0, 26, 0, 26)
minimizeButton.Position = UDim2.new(1, -29, 0, 3)
minimizeButton.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
minimizeButton.Text = "_"
minimizeButton.TextColor3 = Color3.fromRGB(200, 200, 200)
minimizeButton.TextSize = 14
minimizeButton.Font = Enum.Font.GothamBold
minimizeButton.BorderSizePixel = 0
minimizeButton.Parent = topBar

local minimizeCorner = Instance.new("UICorner")
minimizeCorner.CornerRadius = UDim.new(0, 5)
minimizeCorner.Parent = minimizeButton

-- 内容容器 (可滑动)
local contentFrame = Instance.new("ScrollingFrame")
contentFrame.Name = "ContentFrame"
contentFrame.Size = UDim2.new(1, -10, 1, -38)
contentFrame.Position = UDim2.new(0, 5, 0, 36)
contentFrame.BackgroundTransparency = 1
contentFrame.BorderSizePixel = 0
contentFrame.ScrollBarThickness = 3
contentFrame.ScrollBarImageColor3 = Color3.fromRGB(100, 100, 100)
contentFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
contentFrame.ScrollingDirection = Enum.ScrollingDirection.Y
contentFrame.AutomaticCanvasSize = Enum.AutomaticSize.Y
contentFrame.ElasticBehavior = Enum.ElasticBehavior.WhenScrollable
contentFrame.ScrollingEnabled = true
contentFrame.Parent = mainFrame

local contentLayout = Instance.new("UIListLayout")
contentLayout.Padding = UDim.new(0, 5)
contentLayout.SortOrder = Enum.SortOrder.LayoutOrder
contentLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
contentLayout.Parent = contentFrame

-- 按钮数据
local buttons = {
    {
        name = "Dex Explorer", 
        code = 'loadstring(game:HttpGet("https://raw.githubusercontent.com/infyiff/backup/main/dex.lua"))()',
        type = "execute"
    },
    {
        name = "Infinite Yield", 
        code = 'loadstring(game:HttpGet("https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source"))()',
        type = "execute"
    },
    {
        name = "Simple Spy", 
        code = 'loadstring(game:HttpGet("https://raw.githubusercontent.com/78n/SimpleSpy/main/SimpleSpySource.lua"))()',
        type = "execute"
    },
    {
        name = "Sigma Spy", 
        code = 'loadstring(game:HttpGet("https://haxhell.com/raw/universal-script-sigma-spy-or-remote-spy-script-builder-decompiler"))()',
        type = "execute"
    },
    {
        name = "Console", 
        code = [[local vim = game:GetService("VirtualInputManager")
vim:SendKeyEvent(true, Enum.KeyCode.F9, false, game)
vim:SendKeyEvent(false, Enum.KeyCode.F9, false, game)
print("F9已模拟按下！")]],
        type = "execute"
    },
    {
        name = "脚本UI库", 
        code = "https://github.com/weakhoes/Roblox-UI-Libs",
        type = "copy"
    },
    {
        name = "xiaoyun", 
        code = "https://discord.gg/43NMC3sWC",
        type = "copy",
        rainbow = true
    }
}

local rainbowElements = {}

-- 创建按钮函数
local function createButton(data, index)
    local button = Instance.new("TextButton")
    button.Name = data.name
    button.Size = UDim2.new(1, -6, 0, 36)
    button.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
    button.Text = ""
    button.BorderSizePixel = 0
    button.AutoButtonColor = false
    button.LayoutOrder = index
    button.Parent = contentFrame
    
    local buttonCorner = Instance.new("UICorner")
    buttonCorner.CornerRadius = UDim.new(0, 6)
    buttonCorner.Parent = button
    
    local buttonBorder = Instance.new("UIStroke")
    buttonBorder.Color = Color3.fromRGB(35, 35, 35)
    buttonBorder.Thickness = 1
    buttonBorder.Parent = button
    
    -- 按钮文本
    local buttonText = Instance.new("TextLabel")
    buttonText.Size = UDim2.new(1, -60, 1, 0)
    buttonText.Position = UDim2.new(0, 8, 0, 0)
    buttonText.BackgroundTransparency = 1
    buttonText.Text = data.name
    buttonText.TextColor3 = Color3.fromRGB(240, 240, 240)
    buttonText.TextSize = 11
    buttonText.Font = Enum.Font.GothamSemibold
    buttonText.TextXAlignment = Enum.TextXAlignment.Left
    buttonText.TextTruncate = Enum.TextTruncate.AtEnd
    buttonText.Parent = button
    
    -- 类型标签
    local typeLabel = Instance.new("TextLabel")
    typeLabel.Size = UDim2.new(0, 44, 0, 18)
    typeLabel.Position = UDim2.new(1, -48, 0.5, -9)
    typeLabel.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    typeLabel.Text = data.type == "copy" and "复制" or "执行"
    typeLabel.TextColor3 = data.type == "copy" and Color3.fromRGB(100, 200, 255) or Color3.fromRGB(150, 255, 150)
    typeLabel.TextSize = 9
    typeLabel.Font = Enum.Font.GothamBold
    typeLabel.BorderSizePixel = 0
    typeLabel.Parent = button
    
    local typeLabelCorner = Instance.new("UICorner")
    typeLabelCorner.CornerRadius = UDim.new(0, 4)
    typeLabelCorner.Parent = typeLabel
    
    -- 如果是彩虹按钮，添加到彩虹元素列表
    if data.rainbow then
        table.insert(rainbowElements, {type = "border", element = buttonBorder, offset = index * 0.1})
        table.insert(rainbowElements, {type = "text", element = buttonText, offset = index * 0.1})
    end
    
    -- 悬停效果
    button.MouseEnter:Connect(function()
        TweenService:Create(button, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(30, 30, 30)}):Play()
        if not data.rainbow then
            TweenService:Create(buttonBorder, TweenInfo.new(0.2), {Color = Color3.fromRGB(60, 60, 60)}):Play()
        end
    end)
    
    button.MouseLeave:Connect(function()
        TweenService:Create(button, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(20, 20, 20)}):Play()
        if not data.rainbow then
            TweenService:Create(buttonBorder, TweenInfo.new(0.2), {Color = Color3.fromRGB(35, 35, 35)}):Play()
        end
    end)
    
    -- 点击效果和执行
    button.MouseButton1Click:Connect(function()
        -- 点击动画
        TweenService:Create(button, TweenInfo.new(0.1), {BackgroundColor3 = Color3.fromRGB(40, 40, 40)}):Play()
        TweenService:Create(typeLabel, TweenInfo.new(0.1), {BackgroundColor3 = Color3.fromRGB(50, 50, 50)}):Play()
        wait(0.1)
        TweenService:Create(button, TweenInfo.new(0.1), {BackgroundColor3 = Color3.fromRGB(30, 30, 30)}):Play()
        TweenService:Create(typeLabel, TweenInfo.new(0.1), {BackgroundColor3 = Color3.fromRGB(30, 30, 30)}):Play()
        
        if data.type == "copy" then
            -- 复制功能
            local success = pcall(function()
                setclipboard(data.code)
            end)
            
            if success then
                notify("✓ 已复制链接")
                -- 短暂显示反馈
                typeLabel.Text = "✓"
                wait(1)
                typeLabel.Text = "复制"
            else
                notify("✗ 复制失败")
            end
        else
            -- 执行代码
            typeLabel.Text = "..."
            
            local success, err = pcall(function()
                loadstring(data.code)()
            end)
            
            -- 更新状态
            if success then
                typeLabel.Text = "✓"
                notify("✓ " .. data.name .. " 已执行")
                wait(1)
                typeLabel.Text = "执行"
            else
                typeLabel.Text = "✗"
                notify("✗ 执行失败")
                warn("执行错误:", err)
                wait(1)
                typeLabel.Text = "执行"
            end
        end
    end)
    
    return button
end

-- 创建所有按钮
for i, buttonData in ipairs(buttons) do
    createButton(buttonData, i)
end

-- 彩虹循环
spawn(function()
    while true do
        for _, item in ipairs(rainbowElements) do
            if item.element and item.element.Parent then
                local color = getRainbowColor(item.offset)
                if item.type == "border" then
                    item.element.Color = color
                elseif item.type == "text" then
                    item.element.TextColor3 = color
                end
            end
        end
        wait(0.03) -- 更新频率
    end
end)

-- 主开关功能
local guiVisible = false
toggleButton.MouseButton1Click:Connect(function()
    guiVisible = not guiVisible
    mainFrame.Visible = guiVisible
    
    -- 开关动画
    if guiVisible then
        TweenService:Create(toggleButton, TweenInfo.new(0.3), {ImageTransparency = 0.3, Rotation = 90}):Play()
        
        -- 主面板入场动画
        mainFrame.Size = UDim2.new(0, 0, 0, 0)
        mainFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
        TweenService:Create(mainFrame, TweenInfo.new(0.35, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
            Size = UDim2.new(0, 180, 0, 240),
            Position = UDim2.new(0.5, -90, 0.5, -120)
        }):Play()
    else
        TweenService:Create(toggleButton, TweenInfo.new(0.3), {ImageTransparency = 0, Rotation = 0}):Play()
    end
end)

-- 最小化按钮功能
minimizeButton.MouseButton1Click:Connect(function()
    guiVisible = false
    mainFrame.Visible = false
    TweenService:Create(toggleButton, TweenInfo.new(0.3), {ImageTransparency = 0, Rotation = 0}):Play()
end)

-- 最小化按钮悬停效果
minimizeButton.MouseEnter:Connect(function()
    TweenService:Create(minimizeButton, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(40, 40, 40)}):Play()
end)

minimizeButton.MouseLeave:Connect(function()
    TweenService:Create(minimizeButton, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(25, 25, 25)}):Play()
end)

-- 开关按钮悬停效果
toggleButton.MouseEnter:Connect(function()
    if not guiVisible then
        TweenService:Create(toggleButton, TweenInfo.new(0.2), {ImageTransparency = 0.2}):Play()
    end
end)

toggleButton.MouseLeave:Connect(function()
    if not guiVisible then
        TweenService:Create(toggleButton, TweenInfo.new(0.2), {ImageTransparency = 0}):Play()
    end
end)

-- 通知函数
function notify(text)
    local notification = Instance.new("Frame")
    notification.Size = UDim2.new(0, 160, 0, 38)
    notification.Position = UDim2.new(0.5, -80, 0, -45)
    notification.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
    notification.BorderSizePixel = 0
    notification.Parent = screenGui
    
    local notifCorner = Instance.new("UICorner")
    notifCorner.CornerRadius = UDim.new(0, 6)
    notifCorner.Parent = notification
    
    local notifBorder = Instance.new("UIStroke")
    notifBorder.Color = Color3.fromRGB(40, 40, 40)
    notifBorder.Thickness = 1
    notifBorder.Parent = notification
    
    local notifText = Instance.new("TextLabel")
    notifText.Size = UDim2.new(1, -16, 1, 0)
    notifText.Position = UDim2.new(0, 8, 0, 0)
    notifText.BackgroundTransparency = 1
    notifText.Text = text
    notifText.TextColor3 = Color3.fromRGB(240, 240, 240)
    notifText.TextSize = 11
    notifText.Font = Enum.Font.Gotham
    notifText.TextWrapped = true
    notifText.Parent = notification
    
    TweenService:Create(notification, TweenInfo.new(0.25), {Position = UDim2.new(0.5, -80, 0, 8)}):Play()
    
    wait(2)
    
    TweenService:Create(notification, TweenInfo.new(0.25), {Position = UDim2.new(0.5, -80, 0, -45)}):Play()
    wait(0.25)
    notification:Destroy()
end

-- 启动通知
notify("✓ 工具中心已加载")

print("脚本工具中心已加载! 点击左侧图标打开界面")
