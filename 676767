local Rayfield = loadstring(game:HttpGet(
    "https://sirius.menu/gen2"
))()

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

local Window = Rayfield:CreateWindow({
    name = "LQHZ HUB",
    subtitle = "!?⭕⭕?!",
    theme = "ember",
    sidebarLayout = true,
})

-- =========================================================
-- 加载通知
-- =========================================================

Window:Notify({
    title = "已加载猎奇回战AUT脚本",
    content = "用完发现存款被⭕光光\nQQ群 - 1736731564",
    duration = 6,
})

-- =========================================================
-- GLOBAL FARM VARIABLES
-- =========================================================

local Farming = false
local SelectedMobType = "Curses"
local FarmConnection = nil

-- =========================================================
-- 警觉（悲）
-- =========================================================

local PlayerDetectionEnabled = false
local PlayerAddedConnection = nil

-- =========================================================
-- 立即退出游戏
-- =========================================================

local function LeaveGame()
    pcall(function()
        game:Shutdown()
    end)
end

-- =========================================================
-- 开始检测玩家
-- =========================================================

local function StartPlayerDetection()

    if PlayerAddedConnection then
        PlayerAddedConnection:Disconnect()
        PlayerAddedConnection = nil
    end

    PlayerAddedConnection =
        Players.PlayerAdded:Connect(function(Player)

            if not PlayerDetectionEnabled then
                return
            end

            -- 不检测自己
            if Player == LocalPlayer then
                return
            end

            LeaveGame()

        end)

end

-- =========================================================
-- 停止检测玩家
-- =========================================================

local function StopPlayerDetection()

    if PlayerAddedConnection then
        PlayerAddedConnection:Disconnect()
        PlayerAddedConnection = nil
    end

end

-- =========================================================
-- 刷点小怪
-- =========================================================

local Page1 = Window:CreateTab({
    name = "刷点小怪",
    icon = 1168935652,
})

-- =========================================================
-- 小怪等待位置
-- =========================================================

local SpawnLocations = {

    ["Curses"] = Vector3.new(
        -294.8493347167969,
        205.26832580566406,
        423.2856750488281
    ),

}

-- =========================================================
-- Curses
-- =========================================================

local MobGroups = {

    ["Curses"] = {

        "Roppongi Curse",
        "Mantis Curse",
        "Jujutsu Sorcerer",
        "Fly Head",
        "Flyhead",

    },

}

-- =========================================================
-- 获取角色
-- =========================================================

local function GetCharacter()

    local Character = LocalPlayer.Character

    if not Character then
        return nil, nil
    end

    local Root =
        Character:FindFirstChild("HumanoidRootPart")

    return Character, Root

end

-- =========================================================
-- 找小怪
-- =========================================================

local function FindSpawnedMob()

    local Group = MobGroups[SelectedMobType]

    if not Group then
        return nil
    end

    for _, MobName in ipairs(Group) do

        for _, Object in ipairs(workspace:GetDescendants()) do

            if Object.Name == MobName then

                local Model = Object

                if Object:IsA("BasePart") then

                    Model =
                        Object:FindFirstAncestorOfClass("Model")

                end

                if Model and Model:IsA("Model") then

                    local Humanoid =
                        Model:FindFirstChildOfClass("Humanoid")

                    if not Humanoid
                    or Humanoid.Health > 0 then

                        return Model

                    end

                end

            end

        end

    end

    return nil

end

-- =========================================================
-- 检查小怪是否还活着
-- =========================================================

local function IsMobAlive(Mob)

    if not Mob then
        return false
    end

    if not Mob.Parent then
        return false
    end

    local Humanoid =
        Mob:FindFirstChildOfClass("Humanoid")

    if Humanoid and Humanoid.Health <= 0 then
        return false
    end

    return true

end

-- =========================================================
-- 前往等待位置并悬浮
-- =========================================================

local function GoToSpawnPoint()

    local Character, Root =
        GetCharacter()

    if not Root then
        return
    end

    local Position =
        SpawnLocations[SelectedMobType]

    if not Position then
        return
    end

    Root.Anchored = true

    Root.CFrame =
        CFrame.new(Position)

end

-- =========================================================
-- 悬浮在小怪身边
-- =========================================================

local function FloatAtMob(Mob)

    local Character, Root =
        GetCharacter()

    if not Root or not Mob then
        return
    end

    if not Mob.Parent then
        return
    end

    -- 一直锁住角色，防止掉下来

    Root.Anchored = true

    -- 跟随小怪的位置

    Root.CFrame =
        Mob:GetPivot()

end

-- =========================================================
-- M1 / Rush Attack
-- =========================================================

local function UseM1()

    local Character = LocalPlayer.Character

    if not Character then
        return
    end

    for _, Object in ipairs(Character:GetChildren()) do

        if Object:IsA("Tool") then

            local ToolName =
                string.lower(Object.Name)

            if ToolName == "m1"
            or ToolName == "rush attack"
            or ToolName:find("rush") then

                pcall(function()

                    Object:Activate()

                end)

                return

            end

        end

    end

end

-- =========================================================
-- 停止刷怪
-- =========================================================

local function StopFarm()

    Farming = false

    if FarmConnection then

        FarmConnection:Disconnect()
        FarmConnection = nil

    end

    local Character, Root =
        GetCharacter()

    if Root then
        Root.Anchored = false
    end

end

-- =========================================================
-- 开始刷怪
-- =========================================================

local function StartFarm()

    StopFarm()

    Farming = true

    FarmConnection =
        RunService.Heartbeat:Connect(function()

            if not Farming then
                return
            end

            local Character, Root =
                GetCharacter()

            if not Character or not Root then
                return
            end

            -- =================================================
            -- 检查小怪
            -- =================================================

            local Mob =
                FindSpawnedMob()

            -- =================================================
            -- 没有小怪
            -- =================================================

            if not Mob then

                GoToSpawnPoint()

                return

            end

            -- =================================================
            -- 小怪存在
            -- =================================================

            if IsMobAlive(Mob) then

                FloatAtMob(Mob)

                UseM1()

            else

                Root.Anchored = false

            end

        end)

end

-- =========================================================
-- 小怪列表
-- =========================================================

Page1:CreateDropdown({

    name = "小怪列表",

    description = "选择刷怪类型",

    options = {
        "Curses",
    },

    value = "Curses",

    multiSelect = false,

    callback = function(Value)

        SelectedMobType = Value

        if Farming then
            StartFarm()
        end

    end,

})

-- =========================================================
-- 开刷
-- =========================================================

Page1:CreateToggle({

    name = "开刷",

    description = "等待小怪生成并自动攻击",

    value = false,

    callback = function(Value)

        if Value then

            StartFarm()

        else

            StopFarm()

        end

    end,

})

-- =========================================================
-- 搞个特质
-- =========================================================

local Page2 = Window:CreateTab({

    name = "搞个特质",

    icon = 1168935652,

})

Page2:CreateButton({

    name = "测试按钮",

    description = "特质功能以后放这里",

    callback = function()

        print("搞个特质")

    end,

})

-- =========================================================
-- 整点物品
-- =========================================================

local Page3 = Window:CreateTab({

    name = "整点物品",

    icon = 1168935652,

})

Page3:CreateButton({

    name = "测试按钮",

    description = "物品功能以后放这里",

    callback = function()

        print("整点物品")

    end,

})

-- =========================================================
-- 整点皮肤
-- =========================================================

local Page4 = Window:CreateTab({

    name = "整点皮肤",

    icon = 1168935652,

})

Page4:CreateButton({

    name = "测试按钮",

    description = "皮肤功能以后放这里",

    callback = function()

        print("整点皮肤")

    end,

})

-- =========================================================
-- 去个位置
-- =========================================================

local Page5 = Window:CreateTab({

    name = "去个位置",

    icon = 1168935652,

})

local Locations = {

    ["大树"] = Vector3.new(
        -1496.3499755859375,
        35.84998321533203,
        -865.602783203125
    ),

    ["沙漠"] = Vector3.new(
        -285.27508544921875,
        153.63967895507812,
        434.0932922363281
    ),

    ["涩谷"] = Vector3.new(
        -4545.57275390625,
        50.16208267211914,
        1594.32275390625
    ),

    ["普奇神父"] = Vector3.new(
        -2193.07080078125,
        -9.17918586730957,
        -516.69873046875
    ),

    ["斗兽场"] = Vector3.new(
        -2295.759765625,
        -8.740321159362793,
        -809.8660278320312
    ),

    ["Boss场地1"] = Vector3.new(
        -976.3657836914062,
        -132.6921844482422,
        521.2234497070312
    ),

    ["Boss场地2"] = Vector3.new(
        27310.802734375,
        -206.3090362548828,
        12823.291015625
    ),

    ["实验室门口"] = Vector3.new(
        749.8814086914062,
        -26.33804702758789,
        1172.1077880859375
    ),

    ["后室"] = Vector3.new(
        0,
        0,
        0
    ),

    ["港口"] = Vector3.new(
        -1469.6943359375,
        -11.700063705444336,
        -1493.6536865234375
    ),

    ["浮空村"] = Vector3.new(
        -993.657958984375,
        174.4108123779297,
        1275.4114990234375
    ),

    ["骑士洞穴"] = Vector3.new(
        -331.3799133300781,
        -34.695350646972656,
        577.0524291992188
    ),

    ["地下车站"] = Vector3.new(
        -3953.00732421875,
        -88.52450561523438,
        668.8258666992188
    ),

    ["PVP（篮球场）"] = Vector3.new(
        -927.177001953125,
        90.21000671386719,
        -13.227560043334961
    ),

    ["PVP（监狱）"] = Vector3.new(
        -584.0096435546875,
        89.89999389648438,
        -1271.91259765625
    ),

    ["PVP（尸魂界）"] = Vector3.new(
        -4406.64111328125,
        -10.18994140625,
        108.43235778808594
    ),

    ["乔家大院"] = Vector3.new(
        -1967.45556640625,
        34.849769592285156,
        1630.1724853515625
    ),

    ["橘子镇"] = Vector3.new(
        0,
        0,
        0
    ),

    ["糖浆村"] = Vector3.new(
        0,
        0,
        0
    ),

    ["海军基地"] = Vector3.new(
        0,
        0,
        0
    ),

}

for Name, Position in pairs(Locations) do

    Page5:CreateButton({

        name = Name,

        callback = function()

            local Character, Root =
                GetCharacter()

            if Root then

                Root.Anchored = false

                Root.CFrame =
                    CFrame.new(Position)

            end

        end,

    })

end

-- =========================================================
-- 整点别的
-- =========================================================

local Page6 = Window:CreateTab({

    name = "整点别的",

    icon = 1168935652,

})

-- =========================================================
-- 警觉（悲）
-- =========================================================

Page6:CreateToggle({

    name = "检测到玩家进入 立即退出游戏",

    description = "检测到其他玩家加入服务器后立即跑路",

    value = false,

    flag = "PlayerDetection",

    callback = function(Value)

        PlayerDetectionEnabled = Value

        if Value then

            StartPlayerDetection()

        else

            StopPlayerDetection()

        end

    end,

})

Page6:CreateButton({

    name = "点我立刻跑路",

    description = "立即退出当前游戏",

    callback = function()

        LeaveGame()

    end,

})

-- =========================================================
-- Infinite Yield
-- =========================================================

Page6:CreateButton({

    name = "Infinite Yield",

    description = "加载 Infinite Yield",

    callback = function()

        loadstring(game:HttpGet(
            "https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source"
        ))()

    end,

})

-- =========================================================
-- 销毁证据
-- =========================================================

Page6:CreateButton({

    name = "销毁证据",

    description = "销毁 LQHZ HUB",

    callback = function()

        StopFarm()

        StopPlayerDetection()

        Window:Unload()

    end,

})
