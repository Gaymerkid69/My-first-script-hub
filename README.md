
-- By King Harkinian#6680

local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Allah Uakbar Hub", "Ocean")

-- MAIN
local Main = Window:NewTab("Main")
local MainSection = Main:NewSection("Main")


MainSection:NewButton("Esp&Aimbot", "Gives you Esp and aimbot", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/CriShoux/OwlHub/master/OwlHub.txt"))()
end)


MainSection:NewButton("Aimlock", "Aimlock that works on every game", function()
    loadstring(game:HttpGet('https://raw.githubusercontent.com/Stefanuk12/ROBLOX/master/Universal/Aiming/Examples/AimLock.lua'))()
end)


MainSection:NewToggle("SuperHuman", "Gives you immense speed and high jumppower", function(state)
    if state then
        game.Players.LocalPlayer.Character.Humanoid.JumpPower = 120
--SPEED:
game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = 120
    else
        game.Players.LocalPlayer.Character.Humanoid.JumpPower = 50
--SPEED:
game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = 16
    end
end)


MainSection:NewButton("CMD-X", "Gives you admin commands enjoy!", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/CMD-X/CMD-X/master/Source",true))()
end)


--FUN
local Fun = Window:NewTab("Fun")
local FunSection = Fun:NewSection("HaveFUN!")


FunSection:NewButton("Reanimate", "Only use this when your in r15", function()
    --reanimate by MyWorld#4430 discord.gg/pYVHtSJmEY
--"oMg tHIs cODe iS uNReaDabLe sO iT SUckS" -its not a script for u to understand and edit but to use with your other scripts
local v3_net, v3_808 = Vector3.new(0, 25.1, 0), Vector3.new(8, 0, 8)
local function getNetlessVelocity(realPartVelocity)
    local mag = realPartVelocity.Magnitude
    if mag > 1 then
        local unit = realPartVelocity.Unit
        if (unit.Y > 0.25) or (unit.Y < -0.75) then
            return unit * (25.1 / unit.Y)
        end
    end
    return v3_net + realPartVelocity * v3_808
end
local simradius = "shp" --simulation radius (net bypass) method
--"shp" - sethiddenproperty
--"ssr" - setsimulationradius
--false - disable
local simrad = 1000 --simulation radius value
local healthHide = true --moves your head away every 3 seconds so players dont see your health bar (alignmode 4 only)
local reclaim = true --if you lost control over a part this will move your primary part to the part so you get it back (alignmode 4)
local novoid = true --prevents parts from going under workspace.FallenPartsDestroyHeight if you control them (alignmode 4 only)
local physp = nil --PhysicalProperties.new(0.01, 0, 1, 0, 0) --sets .CustomPhysicalProperties to this for each part
local noclipAllParts = false --set it to true if you want noclip
local antiragdoll = true --removes hingeConstraints and ballSocketConstraints from your character
local newanimate = true --disables the animate script and enables after reanimation
local discharscripts = true --disables all localScripts parented to your character before reanimation
local R15toR6 = true --tries to convert your character to r6 if its r15
local hatcollide = true --makes hats cancollide (credit to ShownApe) (works only with reanimate method 0)
local humState16 = true --enables collisions for limbs before the humanoid dies (using hum:ChangeState)
local addtools = false --puts all tools from backpack to character and lets you hold them after reanimation
local hedafterneck = true --disable aligns for head and enable after neck or torso is removed
local loadtime = game:GetService("Players").RespawnTime + 100000 --anti respawn delay
local method = 3 --reanimation method
--methods:
--0 - breakJoints (takes [loadtime] seconds to load)
--1 - limbs
--2 - limbs + anti respawn
--3 - limbs + breakJoints after [loadtime] seconds
--4 - remove humanoid + breakJoints
--5 - remove humanoid + limbs
local alignmode = 4 --AlignPosition mode
--modes:
--1 - AlignPosition rigidity enabled true
--2 - 2 AlignPositions rigidity enabled both true and false
--3 - AlignPosition rigidity enabled false
--4 - no AlignPosition, CFrame only
local flingpart = "HumanoidRootPart" --name of the part or the hat used for flinging
--the fling function
--usage: fling(target, duration, velocity)
--target can be set to: basePart, CFrame, Vector3, character model or humanoid (flings at mouse.Hit if argument not provided)
--duration (fling time in seconds) can be set to a number or a string convertable to a number (0.5s if not provided)
--velocity (fling part rotation velocity) can be set to a vector3 value (Vector3.new(20000, 20000, 20000) if not provided)

local lp = game:GetService("Players").LocalPlayer
local rs, ws, sg = game:GetService("RunService"), game:GetService("Workspace"), game:GetService("StarterGui")
local stepped, heartbeat, renderstepped = rs.Stepped, rs.Heartbeat, rs.RenderStepped
local twait, tdelay, rad, inf, abs, clamp = task.wait, task.delay, math.rad, math.huge, math.abs, math.clamp
local cf, v3, angles = CFrame.new, Vector3.new, CFrame.Angles
local v3_0, cf_0 = v3(0, 0, 0), cf(0, 0, 0)

local c = lp.Character
if not (c and c.Parent) then
return
end

c:GetPropertyChangedSignal("Parent"):Connect(function()
if not (c and c.Parent) then
    c = nil
end
end)

local clone, destroy, getchildren, getdescendants, isa = c.Clone, c.Destroy, c.GetChildren, c.GetDescendants, c.IsA

local function gp(parent, name, className)
if typeof(parent) == "Instance" then
    for i, v in pairs(getchildren(parent)) do
        if (v.Name == name) and isa(v, className) then
            return v
        end
    end
end
return nil
end

local fenv = getfenv()

local shp = fenv.sethiddenproperty or fenv.set_hidden_property or fenv.set_hidden_prop or fenv.sethiddenprop
local ssr = fenv.setsimulationradius or fenv.set_simulation_radius or fenv.set_sim_radius or fenv.setsimradius or fenv.setsimrad or fenv.set_sim_rad

healthHide = healthHide and ((method == 0) or (method == 2) or (method == 3)) and gp(c, "Head", "BasePart")

local reclaim, lostpart = reclaim and c.PrimaryPart, nil

local function align(Part0, Part1)

local att0 = Instance.new("Attachment")
att0.Position, att0.Orientation, att0.Name = v3_0, v3_0, "att0_" .. Part0.Name
local att1 = Instance.new("Attachment")
att1.Position, att1.Orientation, att1.Name = v3_0, v3_0, "att1_" .. Part1.Name

if alignmode == 4 then

    local hide = false
    if Part0 == healthHide then
        healthHide = false
        tdelay(0, function()
            while twait(2.9) and Part0 and c do
                hide = #Part0:GetConnectedParts() == 1
                twait(0.1)
                hide = false
            end
        end)
    end
    
    local rot = rad(0.05)
    local con0, con1 = nil, nil
    con0 = stepped:Connect(function()
        if not (Part0 and Part1) then return con0:Disconnect() and con1:Disconnect() end
        Part0.RotVelocity = Part1.RotVelocity
    end)
    local lastpos = Part0.Position
    con1 = heartbeat:Connect(function(delta)
        if not (Part0 and Part1 and att1) then return con0:Disconnect() and con1:Disconnect() end
        if (not Part0.Anchored) and (Part0.ReceiveAge == 0) then
            if lostpart == Part0 then
                lostpart = nil
            end
            local newcf = Part1.CFrame * att1.CFrame
            if Part1.Velocity.Magnitude > 0.1 then
                Part0.Velocity = getNetlessVelocity(Part1.Velocity)
            else
                local vel = (newcf.Position - lastpos) / delta
                Part0.Velocity = getNetlessVelocity(vel)
                if vel.Magnitude < 1 then
                    rot = -rot
                    newcf *= angles(0, 0, rot)
                end
            end
            lastpos = newcf.Position
            if lostpart and (Part0 == reclaim) then
                newcf = lostpart.CFrame
            elseif hide then
                newcf += v3(0, 3000, 0)
            end
            if novoid and (newcf.Y < ws.FallenPartsDestroyHeight + 0.1) then
                newcf += v3(0, ws.FallenPartsDestroyHeight + 0.1 - newcf.Y, 0)
            end
            Part0.CFrame = newcf
        elseif (not Part0.Anchored) and (abs(Part0.Velocity.X) < 45) and (abs(Part0.Velocity.Y) < 25) and (abs(Part0.Velocity.Z) < 45) then
            lostpart = Part0
        end
    end)

else
    
    Part0.CustomPhysicalProperties = physp
    if (alignmode == 1) or (alignmode == 2) then
        local ape = Instance.new("AlignPosition")
        ape.MaxForce, ape.MaxVelocity, ape.Responsiveness = inf, inf, inf
        ape.ReactionForceEnabled, ape.RigidityEnabled, ape.ApplyAtCenterOfMass = false, true, false
        ape.Attachment0, ape.Attachment1, ape.Name = att0, att1, "AlignPositionRtrue"
        ape.Parent = att0
    end
    
    if (alignmode == 2) or (alignmode == 3) then
        local apd = Instance.new("AlignPosition")
        apd.MaxForce, apd.MaxVelocity, apd.Responsiveness = inf, inf, inf
        apd.ReactionForceEnabled, apd.RigidityEnabled, apd.ApplyAtCenterOfMass = false, false, false
        apd.Attachment0, apd.Attachment1, apd.Name = att0, att1, "AlignPositionRfalse"
        apd.Parent = att0
    end
    
    local ao = Instance.new("AlignOrientation")
    ao.MaxAngularVelocity, ao.MaxTorque, ao.Responsiveness = inf, inf, inf
    ao.PrimaryAxisOnly, ao.ReactionTorqueEnabled, ao.RigidityEnabled = false, false, false
    ao.Attachment0, ao.Attachment1 = att0, att1
    ao.Parent = att0
    
    local con0, con1 = nil, nil
    local vel = Part0.Velocity
    con0 = renderstepped:Connect(function()
        if not (Part0 and Part1) then return con0:Disconnect() and con1:Disconnect() end
        Part0.Velocity = vel
    end)
    local lastpos = Part0.Position
    con1 = heartbeat:Connect(function(delta)
        if not (Part0 and Part1) then return con0:Disconnect() and con1:Disconnect() end
        vel = Part0.Velocity
        if Part1.Velocity.Magnitude > 0.01 then
            Part0.Velocity = getNetlessVelocity(Part1.Velocity)
        else
            Part0.Velocity = getNetlessVelocity((Part0.Position - lastpos) / delta)
        end
        lastpos = Part0.Position
    end)

end

att0:GetPropertyChangedSignal("Parent"):Connect(function()
    Part0 = att0.Parent
    if not isa(Part0, "BasePart") then
        att0 = nil
        if lostpart == Part0 then
            lostpart = nil
        end
        Part0 = nil
    end
end)
att0.Parent = Part0

att1:GetPropertyChangedSignal("Parent"):Connect(function()
    Part1 = att1.Parent
    if not isa(Part1, "BasePart") then
        att1 = nil
        Part1 = nil
    end
end)
att1.Parent = Part1
end

local function respawnrequest()
local ccfr, c = ws.CurrentCamera.CFrame, lp.Character
lp.Character = nil
lp.Character = c
local con = nil
con = ws.CurrentCamera.Changed:Connect(function(prop)
    if (prop ~= "Parent") and (prop ~= "CFrame") then
        return
    end
    ws.CurrentCamera.CFrame = ccfr
    con:Disconnect()
end)
end

local destroyhum = (method == 4) or (method == 5)
local breakjoints = (method == 0) or (method == 4)
local antirespawn = (method == 0) or (method == 2) or (method == 3)

hatcollide = hatcollide and (method == 0)

addtools = addtools and lp:FindFirstChildOfClass("Backpack")

if type(simrad) ~= "number" then simrad = 1000 end
if shp and (simradius == "shp") then
tdelay(0, function()
    while c do
        shp(lp, "SimulationRadius", simrad)
        heartbeat:Wait()
    end
end)
elseif ssr and (simradius == "ssr") then
tdelay(0, function()
    while c do
        ssr(simrad)
        heartbeat:Wait()
    end
end)
end

if antiragdoll then
antiragdoll = function(v)
    if isa(v, "HingeConstraint") or isa(v, "BallSocketConstraint") then
        v.Parent = nil
    end
end
for i, v in pairs(getdescendants(c)) do
    antiragdoll(v)
end
c.DescendantAdded:Connect(antiragdoll)
end

if antirespawn then
respawnrequest()
end

if method == 0 then
twait(loadtime)
if not c then
    return
end
end

if discharscripts then
for i, v in pairs(getdescendants(c)) do
    if isa(v, "LocalScript") then
        v.Disabled = true
    end
end
elseif newanimate then
local animate = gp(c, "Animate", "LocalScript")
if animate and (not animate.Disabled) then
    animate.Disabled = true
else
    newanimate = false
end
end

if addtools then
for i, v in pairs(getchildren(addtools)) do
    if isa(v, "Tool") then
        v.Parent = c
    end
end
end

pcall(function()
settings().Physics.AllowSleep = false
settings().Physics.PhysicsEnvironmentalThrottle = Enum.EnviromentalPhysicsThrottle.Disabled
end)

local OLDscripts = {}

for i, v in pairs(getdescendants(c)) do
if v.ClassName == "Script" then
    OLDscripts[v.Name] = true
end
end

local scriptNames = {}

for i, v in pairs(getdescendants(c)) do
if isa(v, "BasePart") then
    local newName, exists = tostring(i), true
    while exists do
        exists = OLDscripts[newName]
        if exists then
            newName = newName .. "_"    
        end
    end
    table.insert(scriptNames, newName)
    Instance.new("Script", v).Name = newName
end
end

local hum = c:FindFirstChildOfClass("Humanoid")
if hum then
for i, v in pairs(hum:GetPlayingAnimationTracks()) do
    v:Stop()
end
end
c.Archivable = true
local cl = clone(c)
if hum and humState16 then
hum:ChangeState(Enum.HumanoidStateType.Physics)
if destroyhum then
    twait(1.6)
end
end
if destroyhum then
pcall(destroy, hum)
end

if not c then
return
end

local head, torso, root = gp(c, "Head", "BasePart"), gp(c, "Torso", "BasePart") or gp(c, "UpperTorso", "BasePart"), gp(c, "HumanoidRootPart", "BasePart")
if hatcollide then
pcall(destroy, torso)
pcall(destroy, root)
pcall(destroy, c:FindFirstChildOfClass("BodyColors") or gp(c, "Health", "Script"))
end

local model = Instance.new("Model", c)
model:GetPropertyChangedSignal("Parent"):Connect(function()
if not (model and model.Parent) then
    model = nil
end
end)

for i, v in pairs(getchildren(c)) do
if v ~= model then
    if addtools and isa(v, "Tool") then
        for i1, v1 in pairs(getdescendants(v)) do
            if v1 and v1.Parent and isa(v1, "BasePart") then
                local bv = Instance.new("BodyVelocity")
                bv.Velocity, bv.MaxForce, bv.P, bv.Name = v3_0, v3(1000, 1000, 1000), 1250, "bv_" .. v.Name
                bv.Parent = v1
            end
        end
    end
    v.Parent = model
end
end

if breakjoints then
model:BreakJoints()
else
if head and torso then
    for i, v in pairs(getdescendants(model)) do
        if isa(v, "JointInstance") then
            local save = false
            if (v.Part0 == torso) and (v.Part1 == head) then
                save = true
            end
            if (v.Part0 == head) and (v.Part1 == torso) then
                save = true
            end
            if save then
                if hedafterneck then
                    hedafterneck = v
                end
            else
                pcall(destroy, v)
            end
        end
    end
end
if method == 3 then
    task.delay(loadtime, pcall, model.BreakJoints, model)
end
end

cl.Parent = ws
for i, v in pairs(getchildren(cl)) do
v.Parent = c
end
pcall(destroy, cl)

local uncollide, noclipcon = nil, nil
if noclipAllParts then
uncollide = function()
    if c then
        for i, v in pairs(getdescendants(c)) do
            if isa(v, "BasePart") then
                v.CanCollide = false
            end
        end
    else
        noclipcon:Disconnect()
    end
end
else
uncollide = function()
    if model then
        for i, v in pairs(getdescendants(model)) do
            if isa(v, "BasePart") then
                v.CanCollide = false
            end
        end
    else
        noclipcon:Disconnect()
    end
end
end
noclipcon = stepped:Connect(uncollide)
uncollide()

for i, scr in pairs(getdescendants(model)) do
if (scr.ClassName == "Script") and table.find(scriptNames, scr.Name) then
    local Part0 = scr.Parent
    if isa(Part0, "BasePart") then
        for i1, scr1 in pairs(getdescendants(c)) do
            if (scr1.ClassName == "Script") and (scr1.Name == scr.Name) and (not scr1:IsDescendantOf(model)) then
                local Part1 = scr1.Parent
                if (Part1.ClassName == Part0.ClassName) and (Part1.Name == Part0.Name) then
                    align(Part0, Part1)
                    pcall(destroy, scr)
                    pcall(destroy, scr1)
                    break
                end
            end
        end
    end
end
end

for i, v in pairs(getdescendants(c)) do
if v and v.Parent and (not v:IsDescendantOf(model)) then
    if isa(v, "Decal") then
        v.Transparency = 1
    elseif isa(v, "BasePart") then
        v.Transparency = 1
        v.Anchored = false
    elseif isa(v, "ForceField") then
        v.Visible = false
    elseif isa(v, "Sound") then
        v.Playing = false
    elseif isa(v, "BillboardGui") or isa(v, "SurfaceGui") or isa(v, "ParticleEmitter") or isa(v, "Fire") or isa(v, "Smoke") or isa(v, "Sparkles") then
        v.Enabled = false
    end
end
end

if newanimate then
local animate = gp(c, "Animate", "LocalScript")
if animate then
    animate.Disabled = false
end
end

if addtools then
for i, v in pairs(getchildren(c)) do
    if isa(v, "Tool") then
        v.Parent = addtools
    end
end
end

local hum0, hum1 = model:FindFirstChildOfClass("Humanoid"), c:FindFirstChildOfClass("Humanoid")
if hum0 then
hum0:GetPropertyChangedSignal("Parent"):Connect(function()
    if not (hum0 and hum0.Parent) then
        hum0 = nil
    end
end)
end
if hum1 then
hum1:GetPropertyChangedSignal("Parent"):Connect(function()
    if not (hum1 and hum1.Parent) then
        hum1 = nil
    end
end)

ws.CurrentCamera.CameraSubject = hum1
local camSubCon = nil
local function camSubFunc()
    camSubCon:Disconnect()
    if c and hum1 then
        ws.CurrentCamera.CameraSubject = hum1
    end
end
camSubCon = renderstepped:Connect(camSubFunc)
if hum0 then
    hum0:GetPropertyChangedSignal("Jump"):Connect(function()
        if hum1 then
            hum1.Jump = hum0.Jump
        end
    end)
else
    respawnrequest()
end
end

local rb = Instance.new("BindableEvent", c)
rb.Event:Connect(function()
pcall(destroy, rb)
sg:SetCore("ResetButtonCallback", true)
if destroyhum then
    if c then c:BreakJoints() end
    return
end
if model and hum0 and (hum0.Health > 0) then
    model:BreakJoints()
    hum0.Health = 0
end
if antirespawn then
    respawnrequest()
end
end)
sg:SetCore("ResetButtonCallback", rb)

tdelay(0, function()
while c do
    if hum0 and hum1 then
        hum1.Jump = hum0.Jump
    end
    wait()
end
sg:SetCore("ResetButtonCallback", true)
end)

R15toR6 = R15toR6 and hum1 and (hum1.RigType == Enum.HumanoidRigType.R15)
if R15toR6 then
local part = gp(c, "HumanoidRootPart", "BasePart") or gp(c, "UpperTorso", "BasePart") or gp(c, "LowerTorso", "BasePart") or gp(c, "Head", "BasePart") or c:FindFirstChildWhichIsA("BasePart")
if part then
    local cfr = part.CFrame
    local R6parts = { 
        head = {
            Name = "Head",
            Size = v3(2, 1, 1),
            R15 = {
                Head = 0
            }
        },
        torso = {
            Name = "Torso",
            Size = v3(2, 2, 1),
            R15 = {
                UpperTorso = 0.2,
                LowerTorso = -0.73
            }
        },
        root = {
            Name = "HumanoidRootPart",
            Size = v3(2, 2, 1),
            R15 = {
                HumanoidRootPart = 0
            }
        },
        leftArm = {
            Name = "Left Arm",
            Size = v3(1, 2, 1),
            R15 = {
                LeftHand = -0.73,
                LeftLowerArm = -0.174,
                LeftUpperArm = 0.415
            }
        },
        rightArm = {
            Name = "Right Arm",
            Size = v3(1, 2, 1),
            R15 = {
                RightHand = -0.73,
                RightLowerArm = -0.174,
                RightUpperArm = 0.415
            }
        },
        leftLeg = {
            Name = "Left Leg",
            Size = v3(1, 2, 1),
            R15 = {
                LeftFoot = -0.85,
                LeftLowerLeg = -0.29,
                LeftUpperLeg = 0.49
            }
        },
        rightLeg = {
            Name = "Right Leg",
            Size = v3(1, 2, 1),
            R15 = {
                RightFoot = -0.85,
                RightLowerLeg = -0.29,
                RightUpperLeg = 0.49
            }
        }
    }
    for i, v in pairs(getchildren(c)) do
        if isa(v, "BasePart") then
            for i1, v1 in pairs(getchildren(v)) do
                if isa(v1, "Motor6D") then
                    v1.Part0 = nil
                end
            end
        end
    end
    part.Archivable = true
    for i, v in pairs(R6parts) do
        local part = clone(part)
        part:ClearAllChildren()
        part.Name, part.Size, part.CFrame, part.Anchored, part.Transparency, part.CanCollide = v.Name, v.Size, cfr, false, 1, false
        for i1, v1 in pairs(v.R15) do
            local R15part = gp(c, i1, "BasePart")
            local att = gp(R15part, "att1_" .. i1, "Attachment")
            if R15part then
                local weld = Instance.new("Weld")
                weld.Part0, weld.Part1, weld.C0, weld.C1, weld.Name = part, R15part, cf(0, v1, 0), cf_0, "Weld_" .. i1
                weld.Parent = R15part
                R15part.Massless, R15part.Name = true, "R15_" .. i1
                R15part.Parent = part
                if att then
                    att.Position = v3(0, v1, 0)
                    att.Parent = part
                end
            end
        end
        part.Parent = c
        R6parts[i] = part
    end
    local R6joints = {
        neck = {
            Parent = R6parts.torso,
            Name = "Neck",
            Part0 = R6parts.torso,
            Part1 = R6parts.head,
            C0 = cf(0, 1, 0, -1, 0, 0, 0, 0, 1, 0, 1, -0),
            C1 = cf(0, -0.5, 0, -1, 0, 0, 0, 0, 1, 0, 1, -0)
        },
        rootJoint = {
            Parent = R6parts.root,
            Name = "RootJoint" ,
            Part0 = R6parts.root,
            Part1 = R6parts.torso,
            C0 = cf(0, 0, 0, -1, 0, 0, 0, 0, 1, 0, 1, -0),
            C1 = cf(0, 0, 0, -1, 0, 0, 0, 0, 1, 0, 1, -0)
        },
        rightShoulder = {
            Parent = R6parts.torso,
            Name = "Right Shoulder",
            Part0 = R6parts.torso,
            Part1 = R6parts.rightArm,
            C0 = cf(1, 0.5, 0, 0, 0, 1, 0, 1, -0, -1, 0, 0),
            C1 = cf(-0.5, 0.5, 0, 0, 0, 1, 0, 1, -0, -1, 0, 0)
        },
        leftShoulder = {
            Parent = R6parts.torso,
            Name = "Left Shoulder",
            Part0 = R6parts.torso,
            Part1 = R6parts.leftArm,
            C0 = cf(-1, 0.5, 0, 0, 0, -1, 0, 1, 0, 1, 0, 0),
            C1 = cf(0.5, 0.5, 0, 0, 0, -1, 0, 1, 0, 1, 0, 0)
        },
        rightHip = {
            Parent = R6parts.torso,
            Name = "Right Hip",
            Part0 = R6parts.torso,
            Part1 = R6parts.rightLeg,
            C0 = cf(1, -1, 0, 0, 0, 1, 0, 1, -0, -1, 0, 0),
            C1 = cf(0.5, 1, 0, 0, 0, 1, 0, 1, -0, -1, 0, 0)
        },
        leftHip = {
            Parent = R6parts.torso,
            Name = "Left Hip" ,
            Part0 = R6parts.torso,
            Part1 = R6parts.leftLeg,
            C0 = cf(-1, -1, 0, 0, 0, -1, 0, 1, 0, 1, 0, 0),
            C1 = cf(-0.5, 1, 0, 0, 0, -1, 0, 1, 0, 1, 0, 0)
        }
    }
    for i, v in pairs(R6joints) do
        local joint = Instance.new("Motor6D")
        for prop, val in pairs(v) do
            joint[prop] = val
        end
        R6joints[i] = joint
    end
    if hum1 then
        hum1.RigType, hum1.HipHeight = Enum.HumanoidRigType.R6, 0
    end
end
--the default roblox animate script edited and put in one line
local script = gp(c, "Animate", "LocalScript") if not script.Disabled then script:ClearAllChildren() local Torso = gp(c, "Torso", "BasePart") local RightShoulder = gp(Torso, "Right Shoulder", "Motor6D") local LeftShoulder = gp(Torso, "Left Shoulder", "Motor6D") local RightHip = gp(Torso, "Right Hip", "Motor6D") local LeftHip = gp(Torso, "Left Hip", "Motor6D") local Neck = gp(Torso, "Neck", "Motor6D") local Humanoid = c:FindFirstChildOfClass("Humanoid") local pose = "Standing" local currentAnim = "" local currentAnimInstance = nil local currentAnimTrack = nil local currentAnimKeyframeHandler = nil local currentAnimSpeed = 1.0 local animTable = {} local animNames = { idle = { { id = "http://www.roblox.com/asset/?id=180435571", weight = 9 }, { id = "http://www.roblox.com/asset/?id=180435792", weight = 1 } }, walk = { { id = "http://www.roblox.com/asset/?id=180426354", weight = 10 } }, run = { { id = "run.xml", weight = 10 } }, jump = { { id = "http://www.roblox.com/asset/?id=125750702", weight = 10 } }, fall = { { id = "http://www.roblox.com/asset/?id=180436148", weight = 10 } }, climb = { { id = "http://www.roblox.com/asset/?id=180436334", weight = 10 } }, sit = { { id = "http://www.roblox.com/asset/?id=178130996", weight = 10 } }, toolnone = { { id = "http://www.roblox.com/asset/?id=182393478", weight = 10 } }, toolslash = { { id = "http://www.roblox.com/asset/?id=129967390", weight = 10 } }, toollunge = { { id = "http://www.roblox.com/asset/?id=129967478", weight = 10 } }, wave = { { id = "http://www.roblox.com/asset/?id=128777973", weight = 10 } }, point = { { id = "http://www.roblox.com/asset/?id=128853357", weight = 10 } }, dance1 = { { id = "http://www.roblox.com/asset/?id=182435998", weight = 10 }, { id = "http://www.roblox.com/asset/?id=182491037", weight = 10 }, { id = "http://www.roblox.com/asset/?id=182491065", weight = 10 } }, dance2 = { { id = "http://www.roblox.com/asset/?id=182436842", weight = 10 }, { id = "http://www.roblox.com/asset/?id=182491248", weight = 10 }, { id = "http://www.roblox.com/asset/?id=182491277", weight = 10 } }, dance3 = { { id = "http://www.roblox.com/asset/?id=182436935", weight = 10 }, { id = "http://www.roblox.com/asset/?id=182491368", weight = 10 }, { id = "http://www.roblox.com/asset/?id=182491423", weight = 10 } }, laugh = { { id = "http://www.roblox.com/asset/?id=129423131", weight = 10 } }, cheer = { { id = "http://www.roblox.com/asset/?id=129423030", weight = 10 } }, } local dances = {"dance1", "dance2", "dance3"} local emoteNames = { wave = false, point = false, dance1 = true, dance2 = true, dance3 = true, laugh = false, cheer = false} local function configureAnimationSet(name, fileList) if (animTable[name] ~= nil) then for _, connection in pairs(animTable[name].connections) do connection:disconnect() end end animTable[name] = {} animTable[name].count = 0 animTable[name].totalWeight = 0 animTable[name].connections = {} local config = script:FindFirstChild(name) if (config ~= nil) then table.insert(animTable[name].connections, config.ChildAdded:connect(function(child) configureAnimationSet(name, fileList) end)) table.insert(animTable[name].connections, config.ChildRemoved:connect(function(child) configureAnimationSet(name, fileList) end)) local idx = 1 for _, childPart in pairs(config:GetChildren()) do if (childPart:IsA("Animation")) then table.insert(animTable[name].connections, childPart.Changed:connect(function(property) configureAnimationSet(name, fileList) end)) animTable[name][idx] = {} animTable[name][idx].anim = childPart local weightObject = childPart:FindFirstChild("Weight") if (weightObject == nil) then animTable[name][idx].weight = 1 else animTable[name][idx].weight = weightObject.Value end animTable[name].count = animTable[name].count + 1 animTable[name].totalWeight = animTable[name].totalWeight + animTable[name][idx].weight idx = idx + 1 end end end if (animTable[name].count <= 0) then for idx, anim in pairs(fileList) do animTable[name][idx] = {} animTable[name][idx].anim = Instance.new("Animation") animTable[name][idx].anim.Name = name animTable[name][idx].anim.AnimationId = anim.id animTable[name][idx].weight = anim.weight animTable[name].count = animTable[name].count + 1 animTable[name].totalWeight = animTable[name].totalWeight + anim.weight end end end local function scriptChildModified(child) local fileList = animNames[child.Name] if (fileList ~= nil) then configureAnimationSet(child.Name, fileList) end end script.ChildAdded:connect(scriptChildModified) script.ChildRemoved:connect(scriptChildModified) local animator = Humanoid and Humanoid:FindFirstChildOfClass("Animator") or nil if animator then local animTracks = animator:GetPlayingAnimationTracks() for i, track in ipairs(animTracks) do track:Stop(0) track:Destroy() end end for name, fileList in pairs(animNames) do configureAnimationSet(name, fileList) end local toolAnim = "None" local toolAnimTime = 0 local jumpAnimTime = 0 local jumpAnimDuration = 0.3 local toolTransitionTime = 0.1 local fallTransitionTime = 0.3 local jumpMaxLimbVelocity = 0.75 local function stopAllAnimations() local oldAnim = currentAnim if (emoteNames[oldAnim] ~= nil and emoteNames[oldAnim] == false) then oldAnim = "idle" end currentAnim = "" currentAnimInstance = nil if (currentAnimKeyframeHandler ~= nil) then currentAnimKeyframeHandler:disconnect() end if (currentAnimTrack ~= nil) then currentAnimTrack:Stop() currentAnimTrack:Destroy() currentAnimTrack = nil end return oldAnim end local function playAnimation(animName, transitionTime, humanoid) local roll = math.random(1, animTable[animName].totalWeight) local origRoll = roll local idx = 1 while (roll > animTable[animName][idx].weight) do roll = roll - animTable[animName][idx].weight idx = idx + 1 end local anim = animTable[animName][idx].anim if (anim ~= currentAnimInstance) then if (currentAnimTrack ~= nil) then currentAnimTrack:Stop(transitionTime) currentAnimTrack:Destroy() end currentAnimSpeed = 1.0 currentAnimTrack = humanoid:LoadAnimation(anim) currentAnimTrack.Priority = Enum.AnimationPriority.Core currentAnimTrack:Play(transitionTime) currentAnim = animName currentAnimInstance = anim if (currentAnimKeyframeHandler ~= nil) then currentAnimKeyframeHandler:disconnect() end currentAnimKeyframeHandler = currentAnimTrack.KeyframeReached:connect(keyFrameReachedFunc) end end local function setAnimationSpeed(speed) if speed ~= currentAnimSpeed then currentAnimSpeed = speed currentAnimTrack:AdjustSpeed(currentAnimSpeed) end end local function keyFrameReachedFunc(frameName) if (frameName == "End") then local repeatAnim = currentAnim if (emoteNames[repeatAnim] ~= nil and emoteNames[repeatAnim] == false) then repeatAnim = "idle" end local animSpeed = currentAnimSpeed playAnimation(repeatAnim, 0.0, Humanoid) setAnimationSpeed(animSpeed) end end local toolAnimName = "" local toolAnimTrack = nil local toolAnimInstance = nil local currentToolAnimKeyframeHandler = nil local function toolKeyFrameReachedFunc(frameName) if (frameName == "End") then playToolAnimation(toolAnimName, 0.0, Humanoid) end end local function playToolAnimation(animName, transitionTime, humanoid, priority) local roll = math.random(1, animTable[animName].totalWeight) local origRoll = roll local idx = 1 while (roll > animTable[animName][idx].weight) do roll = roll - animTable[animName][idx].weight idx = idx + 1 end local anim = animTable[animName][idx].anim if (toolAnimInstance ~= anim) then if (toolAnimTrack ~= nil) then toolAnimTrack:Stop() toolAnimTrack:Destroy() transitionTime = 0 end toolAnimTrack = humanoid:LoadAnimation(anim) if priority then toolAnimTrack.Priority = priority end toolAnimTrack:Play(transitionTime) toolAnimName = animName toolAnimInstance = anim currentToolAnimKeyframeHandler = toolAnimTrack.KeyframeReached:connect(toolKeyFrameReachedFunc) end end local function stopToolAnimations() local oldAnim = toolAnimName if (currentToolAnimKeyframeHandler ~= nil) then currentToolAnimKeyframeHandler:disconnect() end toolAnimName = "" toolAnimInstance = nil if (toolAnimTrack ~= nil) then toolAnimTrack:Stop() toolAnimTrack:Destroy() toolAnimTrack = nil end return oldAnim end local function onRunning(speed) if speed > 0.01 then playAnimation("walk", 0.1, Humanoid) if currentAnimInstance and currentAnimInstance.AnimationId == "http://www.roblox.com/asset/?id=180426354" then setAnimationSpeed(speed / 14.5) end pose = "Running" else if emoteNames[currentAnim] == nil then playAnimation("idle", 0.1, Humanoid) pose = "Standing" end end end local function onDied() pose = "Dead" end local function onJumping() playAnimation("jump", 0.1, Humanoid) jumpAnimTime = jumpAnimDuration pose = "Jumping" end local function onClimbing(speed) playAnimation("climb", 0.1, Humanoid) setAnimationSpeed(speed / 12.0) pose = "Climbing" end local function onGettingUp() pose = "GettingUp" end local function onFreeFall() if (jumpAnimTime <= 0) then playAnimation("fall", fallTransitionTime, Humanoid) end pose = "FreeFall" end local function onFallingDown() pose = "FallingDown" end local function onSeated() pose = "Seated" end local function onPlatformStanding() pose = "PlatformStanding" end local function onSwimming(speed) if speed > 0 then pose = "Running" else pose = "Standing" end end local function getTool() return c and c:FindFirstChildOfClass("Tool") end local function getToolAnim(tool) for _, c in ipairs(tool:GetChildren()) do if c.Name == "toolanim" and c.className == "StringValue" then return c end end return nil end local function animateTool() if (toolAnim == "None") then playToolAnimation("toolnone", toolTransitionTime, Humanoid, Enum.AnimationPriority.Idle) return end if (toolAnim == "Slash") then playToolAnimation("toolslash", 0, Humanoid, Enum.AnimationPriority.Action) return end if (toolAnim == "Lunge") then playToolAnimation("toollunge", 0, Humanoid, Enum.AnimationPriority.Action) return end end local function moveSit() RightShoulder.MaxVelocity = 0.15 LeftShoulder.MaxVelocity = 0.15 RightShoulder:SetDesiredAngle(3.14 /2) LeftShoulder:SetDesiredAngle(-3.14 /2) RightHip:SetDesiredAngle(3.14 /2) LeftHip:SetDesiredAngle(-3.14 /2) end local lastTick = 0 local function move(time) local amplitude = 1 local frequency = 1 local deltaTime = time - lastTick lastTick = time local climbFudge = 0 local setAngles = false if (jumpAnimTime > 0) then jumpAnimTime = jumpAnimTime - deltaTime end if (pose == "FreeFall" and jumpAnimTime <= 0) then playAnimation("fall", fallTransitionTime, Humanoid) elseif (pose == "Seated") then playAnimation("sit", 0.5, Humanoid) return elseif (pose == "Running") then playAnimation("walk", 0.1, Humanoid) elseif (pose == "Dead" or pose == "GettingUp" or pose == "FallingDown" or pose == "Seated" or pose == "PlatformStanding") then stopAllAnimations() amplitude = 0.1 frequency = 1 setAngles = true end if (setAngles) then local desiredAngle = amplitude * math.sin(time * frequency) RightShoulder:SetDesiredAngle(desiredAngle + climbFudge) LeftShoulder:SetDesiredAngle(desiredAngle - climbFudge) RightHip:SetDesiredAngle(-desiredAngle) LeftHip:SetDesiredAngle(-desiredAngle) end local tool = getTool() if tool and tool:FindFirstChild("Handle") then local animStringValueObject = getToolAnim(tool) if animStringValueObject then toolAnim = animStringValueObject.Value animStringValueObject.Parent = nil toolAnimTime = time + .3 end if time > toolAnimTime then toolAnimTime = 0 toolAnim = "None" end animateTool() else stopToolAnimations() toolAnim = "None" toolAnimInstance = nil toolAnimTime = 0 end end Humanoid.Died:connect(onDied) Humanoid.Running:connect(onRunning) Humanoid.Jumping:connect(onJumping) Humanoid.Climbing:connect(onClimbing) Humanoid.GettingUp:connect(onGettingUp) Humanoid.FreeFalling:connect(onFreeFall) Humanoid.FallingDown:connect(onFallingDown) Humanoid.Seated:connect(onSeated) Humanoid.PlatformStanding:connect(onPlatformStanding) Humanoid.Swimming:connect(onSwimming) game:GetService("Players").LocalPlayer.Chatted:connect(function(msg) local emote = "" if msg == "/e dance" then emote = dances[math.random(1, #dances)] elseif (string.sub(msg, 1, 3) == "/e ") then emote = string.sub(msg, 4) elseif (string.sub(msg, 1, 7) == "/emote ") then emote = string.sub(msg, 8) end if (pose == "Standing" and emoteNames[emote] ~= nil) then playAnimation(emote, 0.1, Humanoid) end end) playAnimation("idle", 0.1, Humanoid) pose = "Standing" tdelay(0, function() while c do local _, time = wait(0.1) if (script.Parent == c) and (not script.Disabled) then move(time) end end end) end 
end

local torso1 = torso
torso = gp(c, "Torso", "BasePart") or ((not R15toR6) and gp(c, torso.Name, "BasePart"))
if (typeof(hedafterneck) == "Instance") and head and torso and torso1 then
local conNeck, conTorso, conTorso1 = nil, nil, nil
local aligns = {}
local function enableAligns()
    conNeck:Disconnect()
    conTorso:Disconnect()
    conTorso1:Disconnect()
    for i, v in pairs(aligns) do
        v.Enabled = true
    end
end
conNeck = hedafterneck.Changed:Connect(function(prop)
    if table.find({"Part0", "Part1", "Parent"}, prop) then
        enableAligns()
    end
end)
conTorso = torso:GetPropertyChangedSignal("Parent"):Connect(enableAligns)
conTorso1 = torso1:GetPropertyChangedSignal("Parent"):Connect(enableAligns)
for i, v in pairs(getdescendants(head)) do
    if isa(v, "AlignPosition") or isa(v, "AlignOrientation") then
        i = tostring(i)
        aligns[i] = v
        v:GetPropertyChangedSignal("Parent"):Connect(function()
            aligns[i] = nil
        end)
        v.Enabled = false
    end
end
end

local flingpart0 = gp(model, flingpart, "BasePart") or gp(gp(model, flingpart, "Accessory"), "Handle", "BasePart")
local flingpart1 = gp(c, flingpart, "BasePart") or gp(gp(c, flingpart, "Accessory"), "Handle", "BasePart")

local fling = function() end
if flingpart0 and flingpart1 then
flingpart0:GetPropertyChangedSignal("Parent"):Connect(function()
    if not (flingpart0 and flingpart0.Parent) then
        flingpart0 = nil
        fling = function() end
    end
end)
flingpart0.Archivable = true
flingpart1:GetPropertyChangedSignal("Parent"):Connect(function()
    if not (flingpart1 and flingpart1.Parent) then
        flingpart1 = nil
        fling = function() end
    end
end)
local att0 = gp(flingpart0, "att0_" .. flingpart0.Name, "Attachment")
local att1 = gp(flingpart1, "att1_" .. flingpart1.Name, "Attachment")
if att0 and att1 then
    att0:GetPropertyChangedSignal("Parent"):Connect(function()
        if not (att0 and att0.Parent) then
            att0 = nil
            fling = function() end
        end
    end)
    att1:GetPropertyChangedSignal("Parent"):Connect(function()
        if not (att1 and att1.Parent) then
            att1 = nil
            fling = function() end
        end
    end)
    local lastfling = nil
    local mouse = lp:GetMouse()
    fling = function(target, duration, rotVelocity)
        if typeof(target) == "Instance" then
            if isa(target, "BasePart") then
                target = target.Position
            elseif isa(target, "Model") then
                target = gp(target, "HumanoidRootPart", "BasePart") or gp(target, "Torso", "BasePart") or gp(target, "UpperTorso", "BasePart") or target:FindFirstChildWhichIsA("BasePart")
                if target then
                    target = target.Position
                else
                    return
                end
            elseif isa(target, "Humanoid") then
                target = target.Parent
                if not (target and isa(target, "Model")) then
                    return
                end
                target = gp(target, "HumanoidRootPart", "BasePart") or gp(target, "Torso", "BasePart") or gp(target, "UpperTorso", "BasePart") or target:FindFirstChildWhichIsA("BasePart")
                if target then
                    target = target.Position
                else
                    return
                end
            else
                return
            end
        elseif typeof(target) == "CFrame" then
            target = target.Position
        elseif typeof(target) ~= "Vector3" then
            target = mouse.Hit
            if target then
                target = target.Position
            else
                return
            end
        end
        if target.Y < ws.FallenPartsDestroyHeight + 5 then
            target = v3(target.X, ws.FallenPartsDestroyHeight + 5, target.Z)
        end
        lastfling = target
        if type(duration) ~= "number" then
            duration = tonumber(duration) or 0.5
        end
        if typeof(rotVelocity) ~= "Vector3" then
            rotVelocity = v3(20000, 20000, 20000)
        end
        if not (target and flingpart0 and flingpart1 and att0 and att1) then
            return
        end
        flingpart0.Archivable = true
        local flingpart = clone(flingpart0)
        flingpart.Transparency = 1
        flingpart.CanCollide = false
        flingpart.Name = "flingpart_" .. flingpart0.Name
        flingpart.Anchored = true
        flingpart.Velocity = v3_0
        flingpart.RotVelocity = v3_0
        flingpart.Position = target
        flingpart:GetPropertyChangedSignal("Parent"):Connect(function()
            if not (flingpart and flingpart.Parent) then
                flingpart = nil
            end
        end)
        flingpart.Parent = flingpart1
        if flingpart0.Transparency > 0.5 then
            flingpart0.Transparency = 0.5
        end
        att1.Parent = flingpart
        local con = nil
        local rotchg = v3(0, rotVelocity.Unit.Y * -1000, 0)
        con = heartbeat:Connect(function(delta)
            if target and (lastfling == target) and flingpart and flingpart0 and flingpart1 and att0 and att1 then
                flingpart.Orientation += rotchg * delta
                flingpart0.RotVelocity = rotVelocity
            else
                con:Disconnect()
            end
        end)
        if alignmode ~= 4 then
            local con = nil
            con = renderstepped:Connect(function()
                if flingpart0 and target then
                    flingpart0.RotVelocity = v3_0
                else
                    con:Disconnect()
                end
            end)
        end
        twait(duration)
        if lastfling ~= target then
            if flingpart then
                if att1 and (att1.Parent == flingpart) then
                    att1.Parent = flingpart1
                end
                pcall(destroy, flingpart)
            end
            return
        end
        target = nil
        if not (flingpart and flingpart0 and flingpart1 and att0 and att1) then
            return
        end
        flingpart0.RotVelocity = v3_0
        att1.Parent = flingpart1
        pcall(destroy, flingpart)
    end
end
end

--lp:GetMouse().Button1Down:Connect(fling) --click fling
end)


FunSection:NewButton("FE r6 animations", "Gives you an fe r6 animations gui", function()
    save = nil
c3 = function(r,g,b) return Color3.new(r/255,g/255,b/255) end
 
--do something ro get save file
 
if not save then
    save = {
        ui = {
            highlightcolor = c3(33, 122, 255);
            errorcolor = c3(255, 0, 0);
            --AnimationPriority colors
            core = c3(65, 65, 65);
            idle = c3(134, 200, 230);
            movement = c3(114, 230, 121);
            action = c3(235, 235, 235);
        };
        preferences = {
            
        };
        custom_animations = {
            template = {
                Title = "";
                AnimationId = "rbxassetid://";
                Image = "rbxassetid://2151539455"; --not required
                Speed = 1;
                Time = 0;
                Weight = 1;
                Loop = false;
                R6 = true;
                Priority = 2; --0, 1, 2, and 1000 are acceptable priorities
            };
        };
    }
end
 
lp = game:GetService("Players").LocalPlayer
m = lp:GetMouse()
 
function getHumanoid()
    if not lp.Character then return nil end
    return lp.Character:FindFirstChildWhichIsA("Humanoid")
end
 
screengui = game:GetObjects("rbxassetid://02159099015")[1]
screengui.Parent = game:GetService("CoreGui")
main = screengui.Topbar.Main
 
mainframe = main.MainFrame
scrollframe = mainframe.ScrollingFrame
items = scrollframe.Items
search = scrollframe.SearchFrame.Search
searchbutton = scrollframe.SearchFrame.ImageLabel.TextButton
searchframe = scrollframe.SearchFrame
 
preview = main.Preview
previewimage = preview.Image
previewtitle = preview.Title
previewdesc = preview.Desc
 
function draggable(gObj)
    local UserInputService = game:GetService("UserInputService")
 
    local gui = gObj
    
    local dragging
    local dragInput
    local dragStart
    local startPos
    
    local function update(input)
        local delta = input.Position - dragStart
        gui.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
    
    gui.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = gui.Position
            
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)
    
    gui.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            dragInput = input
        end
    end)
    
    UserInputService.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            update(input)
        end
    end)
end
function tween(object,style,direction,t,goal)
    local tweenservice = game:GetService("TweenService")
    local tweenInfo = TweenInfo.new(t,Enum.EasingStyle[style],Enum.EasingDirection[direction])
    local tween = tweenservice:Create(object,tweenInfo,goal)
    tween:Play()
    return tween
end
 
draggable(screengui.Topbar)
 
function checkIfStudio()
    return game.Name ~= "Game"
end
 
if not checkIfStudio() then
    print'Client is not in Roblox studio'
    --main.Size = UDim2.new(0.398, 0, 0.477, 0)
end
 
search.Changed:connect(function(p)
    local n = 0
    for i,v in pairs (items:GetChildren()) do
        if v:IsA("TextButton") and not string.find(v.Title.Text:lower(), search.Text:lower()) then
            v.Visible = false
        elseif v:IsA("TextButton") and string.find(v.Title.Text:lower(), search.Text:lower()) then
            v.Visible = true
            n = n + 1
        end
    end
    if p == "Text" then
        if n > 0 then
            tween(searchframe, "Sine", "Out", 0.25, {
                BorderColor3 = save.ui.highlightcolor;
            })
            wait(0.25)
            tween(searchframe, "Sine", "In", 0.5, {
                BorderColor3 = c3(58, 58, 58);
            })
        else
            tween(searchframe, "Sine", "Out", 0.25, {
                BorderColor3 = save.ui.errorcolor;
            })
            wait(0.25)
            tween(searchframe, "Sine", "In", 0.5, {
                BorderColor3 = c3(58, 58, 58);
            })
        end
    end
end)
 
spawn(function()
    while wait(10) do
        --auto-save every 10 seconds
    end
end)
 
cam = workspace.CurrentCamera
 
running = {}
popAnims = {
    armturbine = {
        Title = "Arm Turbine";
        AnimationId = "rbxassetid://259438880";
        Speed = 1.5;
        Time = 0;
        Weight = 1;
        Loop = true;
        R6 = true;
        Priority = 2; --0, 1, 2, and 1000 are acceptable priorities
    };
    weirdsway = {
        Title = "Weird Sway";
        AnimationId = "rbxassetid://248336677";
        Speed = 1;
        Time = 0;
        Weight = 1;
        Loop = true;
        R6 = true;
        Priority = 2; --0, 1, 2, and 1000 are acceptable priorities
    };
    weirdfloat = {
        Title = "Weird Float";
        AnimationId = "rbxassetid://248336459";
        Speed = 1;
        Time = 0;
        Weight = 1;
        Loop = true;
        R6 = true;
        Priority = 2; --0, 1, 2, and 1000 are acceptable priorities
    };
    weirdpose = {
        Title = "Weird Pose";
        AnimationId = "rbxassetid://248336163";
        Speed = 1;
        Time = 0;
        Weight = 1;
        Loop = true;
        R6 = true;
        Priority = 2; --0, 1, 2, and 1000 are acceptable priorities
    };
    penguinslide = {
        Title = "Penguin Slide";
        AnimationId = "rbxassetid://282574440";
        Speed = 1;
        Time = 0;
        Weight = 1;
        Loop = true;
        R6 = true;
        Priority = 2; --0, 1, 2, and 1000 are acceptable priorities
    };
    scream = {
        Title = "Scream";
        AnimationId = "rbxassetid://180611870";
        Speed = 1.5;
        Time = 0;
        Weight = 1;
        Loop = true;
        R6 = true;
        Priority = 2; --0, 1, 2, and 1000 are acceptable priorities
    };
    crouch = {
        Title = "Crouch";
        AnimationId = "rbxassetid://182724289";
        Speed = 1;
        Time = 0;
        Weight = 1;
        Loop = true;
        R6 = true;
        Priority = 2; --0, 1, 2, and 1000 are acceptable priorities
    };
    happydance = {
        Title = "Happy Dance";
        AnimationId = "rbxassetid://248335946";
        Speed = 1;
        Time = 0;
        Weight = 1;
        Loop = true;
        R6 = true;
        Priority = 2; --0, 1, 2, and 1000 are acceptable priorities
    };
    floatinghead = {
        Title = "Floating Head";
        AnimationId = "rbxassetid://121572214";
        Speed = 1;
        Time = 0;
        Weight = 1;
        Loop = true;
        R6 = true;
        Priority = 2; --0, 1, 2, and 1000 are acceptable priorities
    };
    balloonfloat = {
        Title = "Balloon Float";
        AnimationId = "rbxassetid://148840371";
        Speed = 1;
        Time = 0;
        Weight = 1;
        Loop = true;
        R6 = true;
        Priority = 2; --0, 1, 2, and 1000 are acceptable priorities
    };
    pinchnose = {
        Title = "Pinch Nose";
        AnimationId = "rbxassetid://30235165";
        Speed = 1;
        Time = 0;
        Weight = 1;
        Loop = true;
        R6 = true;
        Priority = 2; --0, 1, 2, and 1000 are acceptable priorities
    };
    goal = {
        Title = "Goal!";
        AnimationId = "rbxassetid://28488254";
        Speed = 1;
        Time = 0;
        Weight = 1;
        Loop = true;
        R6 = true;
        Priority = 2; --0, 1, 2, and 1000 are acceptable priorities
    };
    cry = {
        Title = "Cry";
        AnimationId = "rbxassetid://180612465";
        Speed = 0;
        Time = 1.5;
        Weight = 1;
        Loop = true;
        R6 = true;
        Priority = 2; --0, 1, 2, and 1000 are acceptable priorities
    };
    partytime = {
        Title = "Party Time";
        AnimationId = "rbxassetid://33796059";
        Speed = 1;
        Time = 0;
        Weight = 1;
        Loop = true;
        R6 = true;
        Priority = 2; --0, 1, 2, and 1000 are acceptable priorities
    };
    moondance = {
        Title = "Moon Dance";
        AnimationId = "rbxassetid://27789359";
        Speed = 1;
        Time = 0;
        Weight = 1;
        Loop = true;
        R6 = true;
        Priority = 2; --0, 1, 2, and 1000 are acceptable priorities
    };
    insanelegs = {
        Title = "Insane Legs";
        AnimationId = "rbxassetid://87986341";
        Speed = 99;
        Time = 0;
        Weight = 1;
        Loop = true;
        R6 = true;
        Priority = 2; --0, 1, 2, and 1000 are acceptable priorities
    };
    rotation = {
        Title = "Rotation";
        AnimationId = "rbxassetid://136801964";
        Speed = 1;
        Time = 0;
        Weight = 1;
        Loop = true;
        R6 = true;
        Priority = 2; --0, 1, 2, and 1000 are acceptable priorities
    };
    insanerotation = {
        Title = "Insane Rotation";
        AnimationId = "rbxassetid://136801964";
        Speed = 99;
        Time = 0;
        Weight = 1;
        Loop = true;
        R6 = true;
        Priority = 2; --0, 1, 2, and 1000 are acceptable priorities
    };
    roar = {
        Title = "Roar";
        AnimationId = "rbxassetid://163209885";
        Speed = 1;
        Time = 0;
        Weight = 1;
        Loop = true;
        R6 = true;
        Priority = 2; --0, 1, 2, and 1000 are acceptable priorities
    };
    spin = {
        Title = "Spin";
        AnimationId = "rbxassetid://188632011";
        Speed = 1;
        Time = 0;
        Weight = 1;
        Loop = true;
        R6 = true;
        Priority = 2; --0, 1, 2, and 1000 are acceptable priorities
    };
    zombiearms = {
        Title = "Zombie Arms";
        AnimationId = "rbxassetid://183294396";
        Speed = 0;
        Time = 0;
        Weight = 1;
        Loop = true;
        R6 = true;
        Priority = 2; --0, 1, 2, and 1000 are acceptable priorities
    };
    insane = {
        Title = "Insane";
        AnimationId = "rbxassetid://33796059";
        Speed = 99;
        Time = 0;
        Weight = 1;
        Loop = true;
        R6 = true;
        Priority = 2; --0, 1, 2, and 1000 are acceptable priorities
    };
    neckbreak = {
        Title = "Neck Break";
        AnimationId = "rbxassetid://35154961";
        Speed = 0;
        Time = 2;
        Weight = 1;
        Loop = true;
        R6 = true;
        Priority = 2; --0, 1, 2, and 1000 are acceptable priorities
    };
    headdetach = {
        Title = "Head Detach";
        AnimationId = "rbxassetid://35154961";
        Speed = 0;
        Time = 3;
        Weight = 1;
        Loop = true;
        R6 = true;
        Priority = 2; --0, 1, 2, and 1000 are acceptable priorities
    };
    idle = {
        Title = "Idle";
        AnimationId = "rbxassetid://180435571";
        Speed = 1;
        Time = 0;
        Weight = 1;
        Loop = true;
        R6 = true;
        Priority = 2; --0, 1, 2, and 1000 are acceptable priorities
    };
    charleston = {
        Title = "Charleston";
        AnimationId = "rbxassetid://429703734";
        Speed = 1;
        Time = 0;
        Weight = 1;
        Loop = true;
        R6 = true;
        Priority = 2; --0, 1, 2, and 1000 are acceptable priorities
    };
}
robloxOwns = {}
 
ownerOwns = {}
 
customAnims = {}
 
function getOwnedAnimations(userid)
    local httpserv = game:GetService("HttpService")
    local owned = httpserv:GetAsync("https://inventory.roblox.com/v1/users/"..userid.."/inventory/Animation?pageNumber=1&itemsPerPage=10", true)
    return owned
end
 
 
function getAnim(name)
    return popAnims[name] or customAnims[name]
end
function runAnim(info, humanoid)
    local animation = Instance.new("Animation")
    animation.AnimationId = info.AnimationId
    
    local animtrack = humanoid:LoadAnimation(animation)
    table.insert(running,animtrack)
    animtrack.Priority = info.Priority
    animtrack.Looped = info.Loop
    
    animtrack:Play()
    animtrack:AdjustSpeed(info.Speed)
    animtrack:AdjustWeight(info.Weight)
    animtrack.TimePosition = info.Time
    
    animtrack.Stopped:connect(function()
        for i = 1,#running do
            if running[i] == animtrack then
                table.remove(running,i)
            end
        end
    end)
    
    return animtrack
end
 
template = items.Template
template.Parent = nil
 
function clear()
    for i,v in pairs (items:GetChildren()) do
        if v:IsA("TextButton") then
            v:Destroy()
        end
    end
end
 
--[[
    idle = {
        Title = "Idle";
        AnimationId = "rbxassetid://180435571";
        Speed = 1;
        Time = 0;
        Weight = 1;
        Loop = true;
        R6 = true;
        Priority = 2; --0, 1, 2, and 1000 are acceptable priorities
    };
--]]
 
function createbutton(v)
    local temp = template:Clone()
    temp.Parent = items
    temp.Name = v.Title
    temp.Title.Text = v.Title
    temp.Image.Image = v.Image or "rbxassetid://2151539455"
    if temp.Image.Image == "rbxassetid://2151539455" then
        temp.Image.ImageColor3 = (v.Priority == 0 and save.ui.idle) or (v.Priority == 1 and save.ui.movement) or (v.Priority == 2 and save.ui.action) or (v.Priority == 1000 and save.ui.core)
    else
        temp.Image.ImageColor3 = Color3.new(1,1,1)
    end
    temp.LayoutOrder = math.random(1,10000)
    
    temp.Settings.AnimationId.Value = v.AnimationId
    temp.Settings.Loop.Value = v.Loop
    temp.Settings.Priority.Value = v.Priority
    temp.Settings.R6.Value = v.R6
    temp.Settings.Speed.Value = v.Speed
    temp.Settings.Weight.Value = v.Weight
    temp.Settings.Time.Value = v.Time
    
    temp.MouseEnter:connect(function()
        preview.Title.Text = v.Title
        preview.Desc.Text = "Speed: "..tostring(v.Speed).."\nPriority: "..tostring(v.Priority).."\nR6 Rig: "..tostring(v.R6).."\nAnimID: "..tostring(v.AnimationId).."\n\n"..(v.Description or "No description provided")
        
        preview.Image.Image = v.Image or "rbxassetid://2151539455"
        if preview.Image.Image == "rbxassetid://2151539455" then
            preview.Image.ImageColor3 = (v.Priority == 0 and save.ui.idle) or (v.Priority == 1 and save.ui.movement) or (v.Priority == 2 and save.ui.action) or (v.Priority == 1000 and save.ui.core)
        else
            preview.Image.ImageColor3 = Color3.new(1,1,1)
        end
    end)
    temp.MouseButton1Click:connect(function()
        temp.Border.ImageColor3 = save.ui.highlightcolor
        for i,anim in pairs (running) do
            if anim.Animation.AnimationId == v.AnimationId then
                anim:Stop()
                return
            end
        end
        temp.Border.Visible = true
        local rAnim = runAnim(v, getHumanoid())
        rAnim.Stopped:connect(function()
            temp.Border.Visible = false
        end)
    end)
    
    return temp
end
 
dropdown = mainframe.ScrollingFrame.DropdownFrame
elements = dropdown.HoldContentsFrame.Frame.Elements
dropdownenabled = true
 
tween(dropdown.HoldContentsFrame.Frame, "Linear", "In", 0, {
    Position = UDim2.new(0,0,-1,0)
})
dropdown.HoldContentsFrame.Frame.Visible = false
 
dropdowndeactivate = screengui.DropdownDeactivate
dropdowndeactivate.Visible = false
 
function hideddown()
    tween(dropdown.HoldContentsFrame.Frame, "Linear", "In", 0, {
        Position = UDim2.new(0,0,-1,0)
    })
    dropdown.HoldContentsFrame.Frame.Visible = false
    dropdowndeactivate.Visible = false
    dropdownenabled = true
    
    for i,e in pairs (elements:GetChildren()) do
        if e:IsA("TextButton") then
            e.BackgroundColor3 = c3(46,46,46)
        end
    end
end
 
dropdown.MouseButton1Click:connect(function()
    print'ddownclick'
    dropdownenabled = not dropdownenabled
    if dropdownenabled then
        hideddown()
    else
        tween(dropdown.HoldContentsFrame.Frame, "Linear", "In", 0.3, {
            Position = UDim2.new(0,0,0,0)
        })
        dropdown.HoldContentsFrame.Frame.Visible = true
        dropdowndeactivate.Visible = true
    end
end)
 
dropdowndeactivate.MouseButton1Down:connect(function()
    hideddown()
end)
 
for i,v in pairs (elements:GetChildren()) do
    if v:IsA("TextButton") then
        v.MouseEnter:connect(function()
            for i,e in pairs (elements:GetChildren()) do
                if e:IsA("TextButton") then
                    e.BackgroundColor3 = c3(46,46,46)
                end
            end
            v.BackgroundColor3 = save.ui.highlightcolor
        end)
        v.MouseButton1Click:connect(function()
            hideddown()
            dropdown.TextLabel.Text = v.Name
            sort(v.Name)
        end)
    end
end
 
function sort(category)
    clear()
    if category == "Popular" then
        for i,v in pairs (popAnims) do
            local temp = createbutton(v)
        end
    elseif category == "By Roblox" then
        
    end
end
 
game:GetService('RunService').RenderStepped:connect(function()
    items.Parent.CanvasSize = UDim2.new(0,0,0,items.GridLayout.AbsoluteContentSize.Y + 50)
end)
 
sort("Popular")
end)


FunSection:NewButton("Universal FE ScriptHub", "An FE script hub", function()
    loadstring(game:HttpGet('https://raw.githubusercontent.com/Dvrknvss/UniversalFEScriptHub/main/Script'))()
end)


FunSection:NewButton("Ultimate Troling Gui", "A gui that brings lots of joy", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/killermaster9999mega/UTG-V3/main/FE%20UTG%20V3.txt"))()
end)


--Local Player
local Player = Window:NewTab("Player")
local PlayerSection = Player:NewSection("Player")

PlayerSection:NewSlider("Walkspeed", "Adjust your speed", 500, 0, function(s) -- 500 (MaxValue) | 0 (MinValue)
    game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = s
end)


PlayerSection:NewSlider("Jumppower", "Adjust your jumppower", 500, 0, function(s) -- 500 (MaxValue) | 0 (MinValue)
    game.Players.LocalPlayer.Character.Humanoid.JumpPower = s
end)


PlayerSection:NewButton("Reset Ws/Jp", "Resets your walkspeed and jumppower", function()
    game.Players.LocalPlayer.Character.Humanoid.JumpPower = 50
    game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = 16
end)


--Games
local Games = Window:NewTab("Games")
local GamesSection = Games:NewSection("Games")


GamesSection:NewButton("Roblox doors", "MSDoors script!", function()
    loadstring(game:HttpGet(("https://raw.githubusercontent.com/mstudio45/MSDOORS/main/MSDOORS.lua"),true))()
end)


GamesSection:NewButton("Bloxfruits script", "Blox fruits Zen Hub", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/Kaizenofficiall/ZenHub/main/bfmobile", true))()
end)
