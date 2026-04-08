local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()
local VirtualInputManager = game:GetService("VirtualInputManager")

local Window = Rayfield:CreateWindow({
   Name = "AZZZZZO | Revn V3",
   LoadingTitle = "Raven Rock Farm",
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

local function PressE()
    for i = 1, 15 do
        VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.E, false, game)
        task.wait(0.1)
        VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.E, false, game)
        task.wait(0.05)
    end
end

local function DeltaGlide(target, isPickup)
    local char = game.Players.LocalPlayer.Character
    local hrp = char:WaitForChild("HumanoidRootPart")
    local nc = game:GetService("RunService").Stepped:Connect(function()
        for _, v in pairs(char:GetDescendants()) do
            if v:IsA("BasePart") then v.CanCollide = false end
        end
    end)

    local bv = Instance.new("BodyVelocity", hrp)
    bv.MaxForce = Vector3.new(1e6, 1e6, 1e6)
    local bg = Instance.new("BodyGyro", hrp)
    bg.MaxTorque = Vector3.new(1e6, 1e6, 1e6)

    while (Vector2.new(hrp.Position.X, hrp.Position.Z) - Vector2.new(target.X, target.Z)).Magnitude > 5 and _G.AzoFarm do
        local dir = (Vector3.new(target.X, 85, target.Z) - hrp.Position).Unit
        bv.Velocity = dir * 45
        bg.CFrame = CFrame.new(hrp.Position, Vector3.new(target.X, 85, target.Z))
        task.wait()
    end
    
    bv:Destroy() bg:Destroy() nc:Disconnect()
    hrp.CFrame = CFrame.new(target)
    task.wait(0.5)

    if isPickup then
        PressE()
    else
        VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.Space, false, game)
        task.wait(0.1)
        VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.Space, false, game)
    end
end

Tab:CreateToggle({
   Name = "Start Delta Farm",
   CurrentValue = false,
   Callback = function(Value)
      _G.AzoFarm = Value
      task.spawn(function()
         while _G.AzoFarm do
            DeltaGlide(TakePos, true)
            task.wait(1)
            for _, pos in ipairs(DropLocations) do
                if not _G.AzoFarm then break end
                DeltaGlide(pos, false)
                task.wait(2.5)
            end
         end
      end)
   end,
})

