local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()
local VirtualInputManager = game:GetService("VirtualInputManager")

local Window = Rayfield:CreateWindow({
   Name = "AZZZZZO | Revn V3.1 🛠️",
   LoadingTitle = "Fixing Movement Lag...",
   ConfigurationSaving = { Enabled = false }
})

local TakePos = Vector3.new(-37, 71, -3081)
local DropLocations = {
    Vector3.new(-392.1, 73.0, -2660.1),
    Vector3.new(366.9, 73.3, -2804.5),
    Vector3.new(-84.1, 73.0, -2715.4)
}

local Tab = Window:CreateTab("Auto Farm ✅", 4483362458)
_G.AzoFarm = false

-- وظيفة ضغط E سريعة مع كسر التعليق
local function PressE()
    for i = 1, 10 do
        VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.E, false, game)
        task.wait(0.05)
        VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.E, false, game)
    end
end

local function DeltaGlide(target, isPickup)
    if not _G.AzoFarm then return end
    
    local char = game.Players.LocalPlayer.Character
    local hrp = char:WaitForChild("HumanoidRootPart")
    
    -- تعطيل التصادم عشان ما يعلق في الصناديق
    local nc = game:GetService("RunService").Stepped:Connect(function()
        for _, v in pairs(char:GetDescendants()) do
            if v:IsA("BasePart") then v.CanCollide = false end
        end
    end)

    local bv = Instance.new("BodyVelocity", hrp)
    bv.MaxForce = Vector3.new(1e6, 1e6, 1e6)
    bv.Velocity = Vector3.new(0,0,0)
    
    local bg = Instance.new("BodyGyro", hrp)
    bg.MaxTorque = Vector3.new(1e6, 1e6, 1e6)

    -- حركة الطيران (تعديل الارتفاع لضمان عدم التعليق بالأرض)
    while (Vector2.new(hrp.Position.X, hrp.Position.Z) - Vector2.new(target.X, target.Z)).Magnitude > 6 and _G.AzoFarm do
        local dir = (Vector3.new(target.X, 82, target.Z) - hrp.Position).Unit
        bv.Velocity = dir * 50 -- رفعنا السرعة شوي
        bg.CFrame = CFrame.new(hrp.Position, Vector3.new(target.X, 82, target.Z))
        task.wait()
    end
    
    bv:Destroy()
    bg:Destroy()
    nc:Disconnect()
    
    -- تأكيد الوصول
    hrp.CFrame = CFrame.new(target.X, target.Y + 2, target.Z)
    task.wait(0.3)

    if isPickup then
        PressE()
        task.wait(0.5) -- وقت مستقطع عشان اللعبة تسجل إنك شلت الصندوق
    else
        -- ضغطة Space للتسليم
        VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.Space, false, game)
        task.wait(0.1)
        VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.Space, false, game)
        task.wait(0.5)
    end
end

Tab:CreateToggle({
   Name = "Start Optimized Farm",
   CurrentValue = false,
   Callback = function(Value)
      _G.AzoFarm = Value
      task.spawn(function()
         while _G.AzoFarm do
            -- اذهب وخذ الصندوق
            DeltaGlide(TakePos, true)
            
            -- اذهب وسلم الصناديق
            for _, pos in ipairs(DropLocations) do
                if not _G.AzoFarm then break end
                DeltaGlide(pos, false)
            end
            task.wait(0.5)
         end
      end)
   end,
})
