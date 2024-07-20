local player = game.Players.LocalPlayer
local character = player.Character
local humanoid = character:WaitForChild("Humanoid")

-- Auto dodge settings
local dodgeCooldown = 0.1
local dodgeDuration = 0.2
local dodgeSpeed = 10

-- Counter settings
local counterCooldown = 0.5
local counterDuration = 0.5
local counterDamage = 10

-- Create a proximity prompt to detect punches
local proximityPrompt = Instance.new("ProximityPrompt")
proximityPrompt.Parent = character
proximityPrompt.Range = 1.5
proximityPrompt.RequiresLineOfSight = false

-- Function to dodge punches
local function startDodge()
-- Check if dodge is available (not on cooldown)
if tick() - dodgeCooldown < 0 then return end

-- Start dodging
humanoid.WalkSpeed = dodgeSpeed
wait(dodgeDuration)
humanoid.WalkSpeed = 0
dodgeCooldown = tick() + dodgeCooldown
end

-- Function to counter punches
local function startCounter(hit)
-- Check if counter is available (not on cooldown)
if tick() - counterCooldown < 0 then return end

-- Start counter
local damage = Instance.new("Damage")
damage.Parent = hit.Parent
damage.Amount = counterDamage
counterCooldown = tick() + counterCooldown
end

-- Detect punches and dodge/counter
proximityPrompt.Triggered:Connect(function(hit)
-- Check if the hit object is a punch
if hit.Parent:FindFirstChild("Punch") then
startDodge()
startCounter(hit)
end
end)
