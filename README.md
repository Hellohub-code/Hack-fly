local UIS = game:GetService("UserInputService")
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local flying = false
local flySpeed = 50 -- You can adjust the speed here
local flyForce

-- Create a bodyVelocity object to control the character's flight
local function startFlying()
    flying = true
    flyForce = Instance.new("BodyVelocity")
    flyForce.MaxForce = Vector3.new(0, math.huge, 0)
    flyForce.Velocity = Vector3.new(0, flySpeed, 0) -- Ascend speed
    flyForce.Parent = character:FindFirstChild("HumanoidRootPart")
end

-- Function to stop flying
local function stopFlying()
    flying = false
    if flyForce then
        flyForce:Destroy()
    end
end

-- Listen for user input to toggle flying
UIS.InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.F then -- Press "F" to toggle flight
        if not flying then
            startFlying()
        else
            stopFlying()
        end
    end
end)

-- Updates flight to follow the player’s movement direction
game:GetService("RunService").Stepped:Connect(function()
    if flying then
        local moveDirection = humanoid.MoveDirection
        flyForce.Velocity = Vector3.new(moveDirection.X * flySpeed, flyForce.Velocity.Y, moveDirection.Z * flySpeed)
    end
end)
