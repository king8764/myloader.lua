# myloader.lua
-- سكربت شاشة تحميل محسّن مع مؤثرات متقدمة، أمان وإدارة أفضل للموارد
local function loadScreen()
    local success, err = pcall(function()
        -- إزالة أي شاشات متبقية لتفادي التكرار
        for _, gui in ipairs(game.CoreGui:GetChildren()) do
            if gui.Name == "CustomLoadingScreen" or gui.Name == "CustomPowerLogo" or gui.Name == "CustomScriptGui" then
                gui:Destroy()
            end
        end

        -- قسم شاشة التحميل
        local LoadingScreen = Instance.new("ScreenGui")
        LoadingScreen.Name = "CustomLoadingScreen"
        LoadingScreen.IgnoreGuiInset = true
        LoadingScreen.ResetOnSpawn = false
        LoadingScreen.Parent = game:GetService("CoreGui")

        local LoadingLabel = Instance.new("TextLabel")
        LoadingLabel.Size = UDim2.new(0, 260, 0, 50)
        LoadingLabel.Position = UDim2.new(0.5, -130, 0.5, -25)
        LoadingLabel.Text = "Loading... 0%"
        LoadingLabel.Font = Enum.Font.GothamBold
        LoadingLabel.FontSize = Enum.FontSize.Size24
        LoadingLabel.TextColor3 = Color3.new(1,1,1)
        LoadingLabel.BackgroundTransparency = 1
        LoadingLabel.Parent = LoadingScreen

        -- تحميل تدريجي
        for i = 1, 100 do
            LoadingLabel.Text = "Loading... " .. tostring(i) .. "%"
            task.wait(0.016 + math.random() * 0.012) -- تحميل واقعي أكثر
        end

        LoadingScreen:Destroy()

        -- شاشة الشعار الملون
        local PowerGui = Instance.new("ScreenGui")
        PowerGui.Name = "CustomPowerLogo"
        PowerGui.IgnoreGuiInset = true
        PowerGui.ResetOnSpawn = false
        PowerGui.Parent = game:GetService("CoreGui")

        local logoLabel = Instance.new("TextLabel")
        logoLabel.Size = UDim2.new(0, 220, 0, 55)
        logoLabel.Position = UDim2.new(0.5, -110, 0.5, -27)
        logoLabel.Text = "Power"
        logoLabel.Font = Enum.Font.GothamBlack
        logoLabel.FontSize = Enum.FontSize.Size32
        logoLabel.BackgroundTransparency = 1
        logoLabel.TextStrokeTransparency = 0.7
        logoLabel.Parent = PowerGui

        -- مؤثر قوس قزح متحرك
        local tweenService = game:GetService("TweenService")
        local rainbowColors = {
            Color3.fromRGB(255,0,0),     -- أحمر
            Color3.fromRGB(255,127,0),   -- برتقالي
            Color3.fromRGB(255,255,0),   -- أصفر
            Color3.fromRGB(0,255,0),     -- أخضر
            Color3.fromRGB(0,0,255),     -- أزرق
            Color3.fromRGB(139,0,255)    -- بنفسجي
        }
        -- دالة تغيير اللون تدريجياً
        task.spawn(function()
            local idx = 1
            while PowerGui.Parent do
                local nextIdx = (idx % #rainbowColors) + 1
                local tween = tweenService:Create(logoLabel, TweenInfo.new(0.5, Enum.EasingStyle.Linear), {TextColor3 = rainbowColors[nextIdx]})
                tween:Play()
                tween.Completed:Wait()
                idx = nextIdx
            end
        end)

        task.wait(3)
        PowerGui:Destroy()

        -- واجهة السكربت
        local ScriptGui = Instance.new("ScreenGui")
        ScriptGui.Name = "CustomScriptGui"
        ScriptGui.IgnoreGuiInset = true
        ScriptGui.ResetOnSpawn = false
        ScriptGui.Parent = game:GetService("CoreGui")

        local ScriptingSection = Instance.new("Frame")
        ScriptingSection.Size = UDim2.new(0, 340, 0, 430)
        ScriptingSection.Position = UDim2.new(0.5, -170, 0.45, -215)
        ScriptingSection.BackgroundColor3 = Color3.fromRGB(40,40,40)
        ScriptingSection.BackgroundTransparency = 0.1
        ScriptingSection.BorderSizePixel = 0
        ScriptingSection.Parent = ScriptGui

        local ScriptingLabel = Instance.new("TextLabel")
        ScriptingLabel.Size = UDim2.new(1, 0, 0, 48)
        ScriptingLabel.Position = UDim2.new(0, 0, 0, 0)
        ScriptingLabel.Text = "Scripting"
        ScriptingLabel.Font = Enum.Font.GothamBold
        ScriptingLabel.FontSize = Enum.FontSize.Size24
        ScriptingLabel.TextColor3 = Color3.new(1,1,1)
        ScriptingLabel.BackgroundTransparency = 1
        ScriptingLabel.Parent = ScriptingSection

        local TextBox = Instance.new("TextBox")
        TextBox.Size = UDim2.new(1, -20, 0, 180)
        TextBox.Position = UDim2.new(0, 10, 0, 56)
        TextBox.MultiLine = true
        TextBox.ClearTextOnFocus = false
        TextBox.Text = "-- اكتب السكربت هنا"
        TextBox.Font = Enum.Font.Code
        TextBox.TextSize = 18
        TextBox.BackgroundColor3 = Color3.fromRGB(26,26,26)
        TextBox.TextColor3 = Color3.new(1,1,1)
        TextBox.TextWrapped = true
        TextBox.Parent = ScriptingSection

        local ExecuteButton = Instance.new("TextButton")
        ExecuteButton.Size = UDim2.new(0, 120, 0, 44)
        ExecuteButton.Position = UDim2.new(0.5, -60, 0, 244)
        ExecuteButton.Text = "Execute"
        ExecuteButton.Font = Enum.Font.GothamBlack
        ExecuteButton.TextSize = 19
        ExecuteButton.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
        ExecuteButton.TextColor3 = Color3.new(1,1,1)
        ExecuteButton.Parent = ScriptingSection

        local ConsoleLabel = Instance.new("TextLabel")
        ConsoleLabel.Size = UDim2.new(1, 0, 0, 26)
        ConsoleLabel.Position = UDim2.new(0, 0, 0, 296)
        ConsoleLabel.Text = "Console"
        ConsoleLabel.Font = Enum.Font.Gotham
        ConsoleLabel.FontSize = Enum.FontSize.Size18
        ConsoleLabel.TextColor3 = Color3.fromRGB(230,230,230)
        ConsoleLabel.BackgroundTransparency = 1
        ConsoleLabel.Parent = ScriptingSection

        local ConsoleOutput = Instance.new("TextBox")
        ConsoleOutput.Size = UDim2.new(1, -20, 0, 80)
        ConsoleOutput.Position = UDim2.new(0, 10, 0, 324)
        ConsoleOutput.BackgroundColor3 = Color3.fromRGB(18,18,18)
        ConsoleOutput.TextColor3 = Color3.fromRGB(0,255,0)
        ConsoleOutput.Font = Enum.Font.Code
        ConsoleOutput.TextSize = 15
        ConsoleOutput.Text = ""
        ConsoleOutput.ClearTextOnFocus = false
        ConsoleOutput.MultiLine = true
        ConsoleOutput.TextWrapped = true
        ConsoleOutput.ReadOnly = true
        ConsoleOutput.Parent = ScriptingSection

        -- تشغيل السكربت من صندوق النص
        ExecuteButton.MouseButton1Click:Connect(function()
            local code = TextBox.Text
            local result, err = pcall(function()
                local f = loadstring(code)
                if f then
                    local out = f()
                    if out ~= nil then
                        ConsoleOutput.Text = "Output: " .. tostring(out)
                    else
                        ConsoleOutput.Text = "Executed successfully."
                    end
                else
                    ConsoleOutput.Text = "Compilation error."
                end
            end)
            if not result then
                ConsoleOutput.Text = "Error: " .. tostring(err)
            end
        end)
    end)
    if not success then
        warn("[LoadScreen Error]: " .. tostring(err))
    end
end

-- لتشغيل الشاشة
loadScreen()
