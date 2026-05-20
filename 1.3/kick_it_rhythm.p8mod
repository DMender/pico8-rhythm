pico-8 cartridge // http://www.pico-8.com
version 42
__lua__
-- beat em 2 the beat 1.3
-- Soft Soft

-- variables
	note = stat(56)
	active = false
	adjust_hits = 0
	player = {
		x = 60,
		y = 80,
		speed = 0.3,
		flipped = 4
	}
	facing_flip = false
	note_x = 0
	beat = 166
	synco = true
	anim_frame = 1
	x = 155
	z = 154
	rate = 24
	once_anim = true
	-- [[
	spawn = {
		{facing_flip = true,
		x = 128,
		y = 70,
		speed = 0.15,
		state = "",
		flipped = 8,
		life = 3
		},
		{facing_flip = true,
		x = 0,
		y = 70,
		speed = 0.15,
		state = "",
		flipped = 8,
		life = 3
		},
		{facing_flip = true,
		x = 144,
		y = 86,
		speed = 0.15,
		state = "",
		flipped = 8,
		life = 3
		},
		{facing_flip = true,
		x = -16,
		y = 86,
		speed = 0.15,
		state = "",
		flipped = 8,
		life = 3
		}
	}--]]
	reserved_right = false
	reserved_left = false
	right_queue = 0
	left_queue = 0
	last_beat = 0
	sprites = {}
	sorted_sprites = {}
	whiff_left = true
	whiff_right = true
	lives = 30
	score = 0
	display_score = 0
	one_punch = false
	last_time = 10
	pink=14
	whiff_count = 0

-- functions
	function distance(x1, y1, x2, y2)
		local dx = x1 - x2
		local dy = y1 - y2
		return flr(sqrt(dx*dx + dy*dy))
	end

-- loops
function _init()
	music()
	pal({[0]=0,5,8,7,128,132,4,143,129,1,140,6,131,3,139,15},1)
	poke(0x5f5f,0x3f)
	pal({[0]=136,133,133,12,12,136,134,134,pink,pink,pink,pink,pink,pink,pink,12},2)
	memset(0x5f72,0b11100000,1)
	memset(0x5f77,0b10000000,1)
	memset(0x5f7f,0b00001111,1)
end

function _update60()
-- ded state
		clover_stun = {
		{
			sprite = 153, --hand shadow
			x = player.x,
			spr_x = player.flipped,
			y = player.y,
			spr_y = -5,
			facing = not facing_flip,
			spr_flip_v = true
		},
		{
			sprite = 153,
			x = player.x,
			spr_x = -player.flipped,
			y = player.y,
			spr_y = 4,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 150, --body shadow
			x = player.x,
			spr_x = -player.flipped,
			y = player.y,
			spr_y = 5,
			facing = not facing_flip,
			spr_flip_v = true
		},
		{
			sprite = 150,
			x = player.x,
			spr_x = player.flipped,
			y = player.y,
			spr_y = 5,
			facing = facing_flip,
			spr_flip_v = true
		},
		{
			sprite = 150,
			x = player.x,
			spr_x = -player.flipped,
			y = player.y,
			spr_y = -3,
			facing = not facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 150,
			x = player.x,
			spr_x = player.flipped,
			y = player.y,
			spr_y = -3,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 134, --hand
			x = player.x,
			spr_x = player.flipped,
			y = player.y,
			spr_y = -5,
			facing = not facing_flip,
			spr_flip_v = true
		},
		{
			sprite = 131, --body
			x = player.x,
			spr_x = 0,
			y = player.y,
			spr_y = 1,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 134, --hand
			x = player.x,
			spr_x = -player.flipped,
			y = player.y,
			spr_y = 4,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 144, --shoe
			x = player.x,
			spr_x = player.flipped,
			y = player.y,
			spr_y = 11,
			facing = not facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 143, --shoe
			x = player.x,
			spr_x = -player.flipped,
			y = player.y,
			spr_y = 11,
			facing = not facing_flip,
			spr_flip_v = false
		}
	}
-- [[dynamic gradient

	chunk = flr(player.y/8)
	if chunk >= 11 then
	pal({[0]=136,133,133,12,12,136,134,134,pink,pink,pink,pink,pink,133,133,12},2)
	elseif chunk >= 10 then
	pal({[0]=136,133,133,12,12,136,134,134,pink,pink,pink,pink,133,133,133,12},2)
	elseif chunk >= 9 then
	pal({[0]=136,133,133,12,12,136,134,134,pink,pink,pink,133,133,133,pink,12},2)
	elseif chunk >= 8 then
	pal({[0]=136,133,133,12,12,136,134,134,pink,pink,133,133,133,pink,pink,12},2)
	end
	palette = "0x5f7" .. sub(tostr(flr((player.y+16)/8)*255,true),4,4)
	bin = ""
	for i=1, 8 do
		if flr(player.y%8) > i then
			bin = "0" .. bin
		else
			bin = "1" .. bin
		end
	end
	bin = "0b" .. bin .. ""
	memset(palette,tonum(bin),1)--]]
-- ded
	if lives == 0 then
		music(-1, 1000)
		clover_state = clover_stun
		if btnp(❎) and stat(54) == -1 then
			run()
		end
	else
-- [[clock
	if time() > last_time and lives < 30 then
		last_time = time() + 10
		lives += 1
	end 
	note = stat(56)--]]

-- [[goblin counter
	if #spawn < time()/40 then
		redcap = {
			facing_flip = true,
			x = -16,
			y = 86,
			speed = 0.15,
			state = "",
			flipped = 8,
			life = 3
		}
		if time()%2 == 0 then
			redcap.x = 128
		end
		add(spawn,redcap)
	end--]]
-- [[movement normalization
	norm = 1
	if (btn(⬆️) or btn(⬇️)) and (btn(⬅️) or btn(➡️)) then
		norm = 1
	else
		norm = 1.4
	end--]]
-- [[clover states
	clover_idle = {
		{
			sprite = 152, --hand shadow
			x = player.x,
			spr_x = player.flipped*3/4,
			y = player.y,
			spr_y = 2,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 152,
			x = player.x,
			spr_x = -player.flipped,
			y = player.y,
			spr_y = 3,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 150, --body shadow
			x = player.x,
			spr_x = -player.flipped,
			y = player.y,
			spr_y = 4,
			facing = not facing_flip,
			spr_flip_v = true
		},
		{
			sprite = 150,
			x = player.x,
			spr_x = player.flipped,
			y = player.y,
			spr_y = 4,
			facing = facing_flip,
			spr_flip_v = true
		},
		{
			sprite = 150,
			x = player.x,
			spr_x = -player.flipped,
			y = player.y,
			spr_y = -4,
			facing = not facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 150,
			x = player.x,
			spr_x = player.flipped,
			y = player.y,
			spr_y = -4,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 133, --hand
			x = player.x,
			spr_x = player.flipped*3/4,
			y = player.y,
			spr_y = 2,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 128, --body
			x = player.x,
			spr_x = 0,
			y = player.y,
			spr_y = 0,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 133, --hand
			x = player.x,
			spr_x = -player.flipped,
			y = player.y,
			spr_y = 3,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 139, --shoe
			x = player.x,
			spr_x = player.flipped,
			y = player.y,
			spr_y = 11,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 138, --shoe
			x = player.x,
			spr_x = -player.flipped,
			y = player.y,
			spr_y = 11,
			facing = facing_flip,
			spr_flip_v = false
		}
	}

	clover_walk = {}
		add(clover_walk,{
			{
				sprite = 152, --hand shadow
				x = player.x,
				spr_x = 0,
				y = player.y,
				spr_y = 5,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 152,
				x = player.x,
				spr_x = -player.flipped/2,
				y = player.y,
				spr_y = 3,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 150, --body shadow
				x = player.x,
				spr_x = -player.flipped,
				y = player.y,
				spr_y = 5,
				facing = not facing_flip,
				spr_flip_v = true
			},
			{
				sprite = 150,
				x = player.x,
				spr_x = player.flipped,
				y = player.y,
				spr_y = 5,
				facing = facing_flip,
				spr_flip_v = true
			},
			{
				sprite = 150,
				x = player.x,
				spr_x = -player.flipped,
				y = player.y,
				spr_y = -3,
				facing = not facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 150,
				x = player.x,
				spr_x = player.flipped,
				y = player.y,
				spr_y = -3,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 133, --hand
				x = player.x,
				spr_x = -player.flipped/2,
				y = player.y,
				spr_y = 3,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 128, --body
				x = player.x,
				spr_x = 0,
				y = player.y,
				spr_y = 1,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 133, --hand
				x = player.x,
				spr_x = 0,
				y = player.y,
				spr_y = 5,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 141, --shoe
				x = player.x,
				spr_x = player.flipped,
				y = player.y,
				spr_y = 11,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 140, --shoe
				x = player.x,
				spr_x = -player.flipped,
				y = player.y,
				spr_y = 11,
				facing = facing_flip,
				spr_flip_v = false
			}
		})
		add(clover_walk,{
			{
				sprite = 152, --hand shadow
				x = player.x,
				spr_x = player.flipped*3/4,
				y = player.y,
				spr_y = 2,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 152,
				x = player.x,
				spr_x = -player.flipped,
				y = player.y,
				spr_y = 3,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 150, --body shadow
				x = player.x,
				spr_x = -player.flipped,
				y = player.y,
				spr_y = 4,
				facing = not facing_flip,
				spr_flip_v = true
			},
			{
				sprite = 150,
				x = player.x,
				spr_x = player.flipped,
				y = player.y,
				spr_y = 4,
				facing = facing_flip,
				spr_flip_v = true
			},
			{
				sprite = 150,
				x = player.x,
				spr_x = -player.flipped,
				y = player.y,
				spr_y = -4,
				facing = not facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 150,
				x = player.x,
				spr_x = player.flipped,
				y = player.y,
				spr_y = -4,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 133, --hand
				x = player.x,
				spr_x = player.flipped*3/4,
				y = player.y,
				spr_y = 2,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 128, --body
				x = player.x,
				spr_x = 0,
				y = player.y,
				spr_y = 0,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 133, --hand
				x = player.x,
				spr_x = -player.flipped,
				y = player.y,
				spr_y = 3,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 143, --shoe
				x = player.x,
				spr_x = player.flipped,
				y = player.y,
				spr_y = 11,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 142, --shoe
				x = player.x,
				spr_x = -player.flipped,
				y = player.y,
				spr_y = 11,
				facing = facing_flip,
				spr_flip_v = false
			}
		})
		add(clover_walk,{
			{
				sprite = 151, --hand shadow
				x = player.x,
				spr_x = player.flipped*3/2,
				y = player.y,
				spr_y = -1,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 153,
				x = player.x,
				spr_x = -player.flipped*3/2,
				y = player.y,
				spr_y = 3,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 150, --body shadow
				x = player.x,
				spr_x = -player.flipped,
				y = player.y,
				spr_y = 2,
				facing = not facing_flip,
				spr_flip_v = true
			},
			{
				sprite = 150,
				x = player.x,
				spr_x = player.flipped,
				y = player.y,
				spr_y = 2,
				facing = facing_flip,
				spr_flip_v = true
			},
			{
				sprite = 150,
				x = player.x,
				spr_x = -player.flipped,
				y = player.y,
				spr_y = -6,
				facing = not facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 150,
				x = player.x,
				spr_x = player.flipped,
				y = player.y,
				spr_y = -6,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 132, --hand
				x = player.x,
				spr_x = player.flipped*3/2,
				y = player.y,
				spr_y = -1,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 128, --body
				x = player.x,
				spr_x = 0,
				y = player.y,
				spr_y = -2,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 134, --hand
				x = player.x,
				spr_x = -player.flipped*3/2,
				y = player.y,
				spr_y = 3,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 145, --shoe
				x = player.x,
				spr_x = player.flipped,
				y = player.y,
				spr_y = 11,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 146,
				x = player.x,
				spr_x = player.flipped,
				y = player.y,
				spr_y = 3,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 144, --shoe
				x = player.x,
				spr_x = -player.flipped,
				y = player.y,
				spr_y = 11,
				facing = facing_flip,
				spr_flip_v = false
			}
		})
		add(clover_walk,{
			{
				sprite = 152, --hand shadow
				x = player.x,
				spr_x = player.flipped*3/4,
				y = player.y,
				spr_y = 2,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 152,
				x = player.x,
				spr_x = -player.flipped,
				y = player.y,
				spr_y = 3,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 150, --body shadow
				x = player.x,
				spr_x = -player.flipped,
				y = player.y,
				spr_y = 4,
				facing = not facing_flip,
				spr_flip_v = true
			},
			{
				sprite = 150,
				x = player.x,
				spr_x = player.flipped,
				y = player.y,
				spr_y = 4,
				facing = facing_flip,
				spr_flip_v = true
			},
			{
				sprite = 150,
				x = player.x,
				spr_x = -player.flipped,
				y = player.y,
				spr_y = -4,
				facing = not facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 150,
				x = player.x,
				spr_x = player.flipped,
				y = player.y,
				spr_y = -4,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 133, --hand
				x = player.x,
				spr_x = player.flipped*3/4,
				y = player.y,
				spr_y = 2,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 128, --body
				x = player.x,
				spr_x = 0,
				y = player.y,
				spr_y = 0,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 133, --hand
				x = player.x,
				spr_x = -player.flipped,
				y = player.y,
				spr_y = 3,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 143, --shoe
				x = player.x,
				spr_x = player.flipped,
				y = player.y,
				spr_y = 11,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 142, --shoe
				x = player.x,
				spr_x = -player.flipped,
				y = player.y,
				spr_y = 11,
				facing = facing_flip,
				spr_flip_v = false
			}
		})

	clover_kick = {
		{
			sprite = 151, --hand shadow
			x = player.x,
			spr_x = player.flipped*5/4,
			y = player.y,
			spr_y = 1,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 152,
			x = player.x,
			spr_x = -player.flipped,
			y = player.y,
			spr_y = 5,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 150, --body shadow
			x = player.x,
			spr_x = -player.flipped,
			y = player.y,
			spr_y = 5,
			facing = not facing_flip,
			spr_flip_v = true
		},
		{
			sprite = 150,
			x = player.x,
			spr_x = player.flipped,
			y = player.y,
			spr_y = 5,
			facing = facing_flip,
			spr_flip_v = true
		},
		{
			sprite = 150,
			x = player.x,
			spr_x = -player.flipped,
			y = player.y,
			spr_y = -3,
			facing = not facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 150,
			x = player.x,
			spr_x = player.flipped,
			y = player.y,
			spr_y = -3,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 132, --hand
			x = player.x,
			spr_x = player.flipped*5/4,
			y = player.y,
			spr_y = 1,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 130, --body
			x = player.x,
			spr_x = 0,
			y = player.y,
			spr_y = 1,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 133, --hand
			x = player.x,
			spr_x = -player.flipped,
			y = player.y,
			spr_y = 5,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 142, --shoe
			x = player.x,
			spr_x = -player.flipped,
			y = player.y,
			spr_y = 11,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 147,
			x = player.x,
			spr_x = player.flipped,
			y = player.y,
			spr_y = 11,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 149, --shoe
			x = player.x,
			spr_x = player.flipped*3,
			y = player.y,
			spr_y = 3,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 146,
			x = player.x,
			spr_x = player.flipped*3,
			y = player.y,
			spr_y = -5,
			facing = facing_flip,
			spr_flip_v = false
		}
	}

	clover_punch = {
		{
			sprite = 152, --hand shadow
			x = player.x,
			spr_x = player.flipped/2,
			y = player.y,
			spr_y = 4,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 133, --hand
			x = player.x,
			spr_x = player.flipped/2,
			y = player.y,
			spr_y = 4,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 150, --body shadow
			x = player.x,
			spr_x = -player.flipped,
			y = player.y,
			spr_y = 4,
			facing = not facing_flip,
			spr_flip_v = true
		},
		{
			sprite = 150,
			x = player.x,
			spr_x = player.flipped,
			y = player.y,
			spr_y = 4,
			facing = facing_flip,
			spr_flip_v = true
		},
		{
			sprite = 150,
			x = player.x,
			spr_x = -player.flipped,
			y = player.y,
			spr_y = -4,
			facing = not facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 150,
			x = player.x,
			spr_x = player.flipped,
			y = player.y,
			spr_y = -4,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 130, --body
			x = player.x,
			spr_x = 0,
			y = player.y,
			spr_y = 0,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 136, --hand
			x = player.x,
			spr_x = player.flipped*2,
			y = player.y,
			spr_y = 0,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 137,
			x = player.x,
			spr_x = player.flipped*4,
			y = player.y,
			spr_y = 0,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 139, --shoe
			x = player.x,
			spr_x = player.flipped,
			y = player.y,
			spr_y = 11,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 138, --shoe
			x = player.x,
			spr_x = -player.flipped,
			y = player.y,
			spr_y = 11,
			facing = facing_flip,
			spr_flip_v = false
		}
	}

	clover_whiff = {
		{
			sprite = 152, --hand shadow
			x = player.x,
			spr_x = -player.flipped*3/4,
			y = player.y,
			spr_y = 3,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 150, --body shadow
			x = player.x,
			spr_x = -player.flipped,
			y = player.y,
			spr_y = 4,
			facing = not facing_flip,
			spr_flip_v = true
		},
		{
			sprite = 150,
			x = player.x,
			spr_x = player.flipped,
			y = player.y,
			spr_y = 4,
			facing = facing_flip,
			spr_flip_v = true
		},
		{
			sprite = 150,
			x = player.x,
			spr_x = -player.flipped,
			y = player.y,
			spr_y = -4,
			facing = not facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 150,
			x = player.x,
			spr_x = player.flipped,
			y = player.y,
			spr_y = -4,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 130, --body
			x = player.x,
			spr_x = 0,
			y = player.y,
			spr_y = 0,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 133, --hand
			x = player.x,
			spr_x = -player.flipped*3/4,
			y = player.y,
			spr_y = 3,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 135, --hand
			x = player.x,
			spr_x = player.flipped*5/2,
			y = player.y,
			spr_y = 1,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 139, --shoe
			x = player.x,
			spr_x = player.flipped,
			y = player.y,
			spr_y = 11,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 138, --shoe
			x = player.x,
			spr_x = -player.flipped,
			y = player.y,
			spr_y = 11,
			facing = facing_flip,
			spr_flip_v = false
		}
	}

	clover_kick_whiff = {
		{
			sprite = 151, --hand shadow
			x = player.x,
			spr_x = player.flipped*5/4,
			y = player.y,
			spr_y = 1,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 152,
			x = player.x,
			spr_x = -player.flipped,
			y = player.y,
			spr_y = 5,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 150, --body shadow
			x = player.x,
			spr_x = -player.flipped,
			y = player.y,
			spr_y = 5,
			facing = not facing_flip,
			spr_flip_v = true
		},
		{
			sprite = 150,
			x = player.x,
			spr_x = player.flipped,
			y = player.y,
			spr_y = 5,
			facing = facing_flip,
			spr_flip_v = true
		},
		{
			sprite = 150,
			x = player.x,
			spr_x = -player.flipped,
			y = player.y,
			spr_y = -3,
			facing = not facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 150,
			x = player.x,
			spr_x = player.flipped,
			y = player.y,
			spr_y = -3,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 132, --hand
			x = player.x,
			spr_x = player.flipped*5/4,
			y = player.y,
			spr_y = 1,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 130, --body
			x = player.x,
			spr_x = 0,
			y = player.y,
			spr_y = 1,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 133, --hand
			x = player.x,
			spr_x = -player.flipped,
			y = player.y,
			spr_y = 5,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 142, --shoe
			x = player.x,
			spr_x = -player.flipped,
			y = player.y,
			spr_y = 11,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 147,
			x = player.x,
			spr_x = player.flipped,
			y = player.y,
			spr_y = 11,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 148, --shoe
			x = player.x,
			spr_x = player.flipped*3,
			y = player.y,
			spr_y = 3,
			facing = facing_flip,
			spr_flip_v = false
		}
	}--]]

-- [[all beats
	if flr(note/rate) !=  last_beat then
		last_beat = flr(note/rate)
		active = false
		synco = false
		anim_frame += 1
		if anim_frame == 5 then
			anim_frame = 1
		end
		if (flr(note/rate))%2 == 0 then
			clover_state = clover_idle
			one_punch = false
			if whiff_count >= 0 then
				clover_state = clover_stun
				whiff_count -= 1
			end
			sfx(2)
			pink=14
		end
		--beat track
		note_x -= 1
		if note_x == -8 then
			note_x = 0
		end
	end
	if (flr(note/rate))%2 == 0 and not btn(🅾️) and not btn(❎) then
		active = true
		if flr(note/rate)%4 == 0 then
			synco = true
		end
	end

-- [[movement
	if adjust_hits <= 12 or btn(🅾️) or btn(❎) and clover_state != clover_stunned then
	else
		clover_state = clover_walk[anim_frame]
		-- up
		if btn(⬆️) and player.y > 66 and clover_state != clover_stunned then
			player.y -= 1 * player.speed * norm
		-- down
		elseif btn(⬇️) and player.y < 95 and clover_state != clover_stunned then
			player.y += 1 * player.speed * norm
		end
		-- left
		if btn(⬅️)  and player.x > 0 and clover_state != clover_stunned then
			player.flipped = -4
			facing_flip = true
			player.x -= 1.7 * player.speed * norm
		-- right
		elseif btn(➡️) and player.x < 112 and clover_state != clover_stunned then
			player.flipped = 4
			facing_flip = false
			player.x += 1.7 * player.speed * norm
		end
	end--]]
-- [[punch
	if btnp(❎) and active and clover_state != clover_stunned and whiff_count < 0 then
		adjust_hits += 1
		whiff_left = false
		whiff_right = false
		beat = 167
		sfx(1)
		active = false
		clover_state = clover_punch
		pink=8
	elseif btnp(❎) and clover_state != clover_stunned then
		adjust_hits += 1
		whiff_left = true
		whiff_right = true
		clover_state = clover_whiff
		whiff_count = 2
		beat = 166
		sfx(3)
		active = false
	end--]]
-- [[kick
	if btnp(🅾️) and active and synco and clover_state != clover_stunned and whiff_count < 0 then
		adjust_hits += 1
		whiff_left = false
		whiff_right = false
		beat = 167
		sfx(1)
		active = false
		clover_state = clover_kick
		pink=12
	elseif btnp(🅾️) and clover_state != clover_stunned then
		adjust_hits += 1
		whiff_left = true
		whiff_right = true
		beat = 166
		sfx(3)
		clover_state = clover_kick_whiff
		whiff_count = 2
		active = false
	end--]]

-- [[tutorial animations
	if adjust_hits <= 12 then
		x = 155
		z = 154
		if active then
			x = 161
		end
		if active and synco then
			z = 160
		end
	else
		x = 0
		z = 0
	end--]]
	
-- [[red caps
	right_queue = 0
	left_queue = 0
	for k,i in pairs(spawn) do
		if i.life <= 0 then
			del(spawn, i)
			score += 1
		end
		if anim_frame == 2 and adjust_hits > 12 and distance(player.x,player.y,i.x,i.y) < 32 then
			i.state = "moving" 
		end
		if i.state != "stunned" and adjust_hits > 12 and distance(player.x,player.y,i.x,i.y) < 32 then
			i.state = "punching"
			if anim_frame == 3 and not one_punch then
				clover_state = clover_stun
				lives -= 1
				sfx(0)
				one_punch = true
			end
		end
		if distance(player.x+8,player.y,i.x,i.y) < 24  and i.x > player.x+8 then
			reserved_right = true
			right_queue += 1
			if not whiff_right and not facing_flip and (btnp(🅾️) or btnp(❎)) then
				i.life -= 1
				i.state = "stunned"
				whiff_right = true
				if btnp(🅾️) then
				 i.x += 12
					i.life -= 1
				end
			end
		elseif distance(player.x,player.y,i.x+8,i.y) < 24  and i.x+8 < player.x then
			reserved_left = true
			left_queue += 1
			if not whiff_left and facing_flip and (btnp(🅾️) or btnp(❎)) then
				i.life -= 1
				i.state = "stunned"
				whiff_left = true
				if btnp(🅾️) then
				 i.x -= 12
					i.life -= 1
				end
			end
		elseif i.x - 4 > player.x + 8 and (distance(player.x+8,player.y,i.x,i.y) < 24 or not reserved_right) and i.state != "stunned" then
			i.x -= 1.7 * i.speed
			i.facing_flip = false
			i.flipped = -8
			i.state = "moving"
			
			if i.y > player.y - 24 + k * 4 then
				i.y -= 1 * i.speed
				i.state = "moving"
			end
			if i.y < player.y - 24 + k * 4 then
				i.y += 1 * i.speed
				i.state = "moving"
			end
		elseif i.x + 8  < player.x - 4  and (distance(player.x,player.y,i.x+8,i.y) < 24 or not reserved_left) and i.state != "stunned" then
			i.x += 1.7 * i.speed
			i.facing_flip = true
			i.flipped = 8
			i.state = "moving"
			
			if i.y > player.y - 24 + k * 4 then
				i.y -= 1 * i.speed
				i.state = "moving"
			end
			if i.y < player.y - 24 + k * 4 then
				i.y += 1 * i.speed
				i.state = "moving"
			end
		elseif i.x + 8 > player.x - 4 and i.x + 8 < player.x + 8 and i.state != "stunned" then
			i.x -= 1.7 * i.speed
			i.facing_flip = true
			i.flipped = 8
			i.state = "moving"
		elseif i.x - 4 < player.x + 16 and i.x >= player.x and i.state != "stunned" then
			i.x += 1.7 * i.speed
			i.facing_flip = false
			i.flipped = -8
			i.state = "moving"
		end
	end
	if right_queue == 0 then
		reserved_right = false
	end
	if left_queue == 0 then
		reserved_left = false
	end

	sprites = {}
	for tile in all(clover_state) do
		add(sprites, tile)
	end
	for redcap in all(spawn) do
		redcap.frame = {}
-- redcap states
		redcap_walk = {}
			add(redcap_walk,{
				{
				sprite = 87,
					x = redcap.x,
					spr_x = redcap.flipped*7/8,
					y = redcap.y,
					spr_y = -8,
					facing = redcap.facing_flip,
					spr_flip_v = false,
				},
				{
					sprite = 88,
					x = redcap.x,
					spr_x = -redcap.flipped/8,
					y = redcap.y,
					spr_y = -8,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 92,
					x = redcap.x,
					spr_x = redcap.flipped,
					y = redcap.y,
					spr_y = 0,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 93,
					x = redcap.x,
					spr_x = 0,
					y = redcap.y,
					spr_y = 0,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 94,
					x = redcap.x,
					spr_x = -redcap.flipped,
					y = redcap.y,
					spr_y = 0,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 101,
					x = redcap.x,
					spr_x = redcap.flipped,
					y = redcap.y,
					spr_y = 8,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 102,
					x = redcap.x,
					spr_x = 0,
					y = redcap.y,
					spr_y = 8,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 103,
					x = redcap.x,
					spr_x = -redcap.flipped,
					y = redcap.y,
					spr_y = 8,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 113,
					x = redcap.x,
					spr_x = redcap.flipped*7/8,
					y = redcap.y,
					spr_y = 16,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 114,
					x = redcap.x,
					spr_x = -redcap.flipped/8,
					y = redcap.y,
					spr_y = 16,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 122,
					x = redcap.x,
					spr_x = redcap.flipped*7/8,
					y = redcap.y,
					spr_y = 24,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 123,
					x = redcap.x,
					spr_x = -redcap.flipped/8,
					y = redcap.y,
					spr_y = 24,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 84,
					x = redcap.x,
					spr_x = -redcap.flipped*9/8,
					y = redcap.y,
					spr_y = 24,
					facing = redcap.facing_flip,
					spr_flip_v = false
				}
			})
			add(redcap_walk,{
				{
					sprite = 87,
					x = redcap.x,
					spr_x = redcap.flipped*7/8,
					y = redcap.y,
					spr_y = -8,
					facing = redcap.facing_flip,
					spr_flip_v = false,
				},
				{
					sprite = 88,
					x = redcap.x,
					spr_x = -redcap.flipped/8,
					y = redcap.y,
					spr_y = -8,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 92,
					x = redcap.x,
					spr_x = redcap.flipped,
					y = redcap.y,
					spr_y = 0,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 93,
					x = redcap.x,
					spr_x = 0,
					y = redcap.y,
					spr_y = 0,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 94,
					x = redcap.x,
					spr_x = -redcap.flipped,
					y = redcap.y,
					spr_y = 0,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 101,
					x = redcap.x,
					spr_x = redcap.flipped,
					y = redcap.y,
					spr_y = 8,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 102,
					x = redcap.x,
					spr_x = 0,
					y = redcap.y,
					spr_y = 8,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 103,
					x = redcap.x,
					spr_x = -redcap.flipped,
					y = redcap.y,
					spr_y = 8,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 115,
					x = redcap.x,
					spr_x = redcap.flipped*7/8,
					y = redcap.y,
					spr_y = 16,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 116,
					x = redcap.x,
					spr_x = -redcap.flipped/8,
					y = redcap.y,
					spr_y = 16,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 124,
					x = redcap.x,
					spr_x = redcap.flipped*3/4,
					y = redcap.y,
					spr_y = 24,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 125,
					x = redcap.x,
					spr_x = -redcap.flipped/4,
					y = redcap.y,
					spr_y = 24,
					facing = redcap.facing_flip,
					spr_flip_v = false
				}
			})
			add(redcap_walk,{
				{
				sprite = 87,
					x = redcap.x,
					spr_x = redcap.flipped*7/8,
					y = redcap.y,
					spr_y = -8,
					facing = redcap.facing_flip,
					spr_flip_v = false,
				},
				{
					sprite = 88,
					x = redcap.x,
					spr_x = -redcap.flipped/8,
					y = redcap.y,
					spr_y = -8,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 92,
					x = redcap.x,
					spr_x = redcap.flipped,
					y = redcap.y,
					spr_y = 0,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 93,
					x = redcap.x,
					spr_x = 0,
					y = redcap.y,
					spr_y = 0,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 94,
					x = redcap.x,
					spr_x = -redcap.flipped,
					y = redcap.y,
					spr_y = 0,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 101,
					x = redcap.x,
					spr_x = redcap.flipped,
					y = redcap.y,
					spr_y = 8,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 102,
					x = redcap.x,
					spr_x = 0,
					y = redcap.y,
					spr_y = 8,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 103,
					x = redcap.x,
					spr_x = -redcap.flipped,
					y = redcap.y,
					spr_y = 8,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 113,
					x = redcap.x,
					spr_x = redcap.flipped*7/8,
					y = redcap.y,
					spr_y = 16,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 114,
					x = redcap.x,
					spr_x = -redcap.flipped/8,
					y = redcap.y,
					spr_y = 16,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 122,
					x = redcap.x,
					spr_x = redcap.flipped*7/8,
					y = redcap.y,
					spr_y = 24,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 123,
					x = redcap.x,
					spr_x = -redcap.flipped/8,
					y = redcap.y,
					spr_y = 24,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 84,
					x = redcap.x,
					spr_x = -redcap.flipped*9/8,
					y = redcap.y,
					spr_y = 24,
					facing = redcap.facing_flip,
					spr_flip_v = false
				}
			})
			add(redcap_walk,{
				{
					sprite = 87,
					x = redcap.x,
					spr_x = redcap.flipped*7/8,
					y = redcap.y,
					spr_y = -8,
					facing = redcap.facing_flip,
					spr_flip_v = false,
				},
				{
					sprite = 88,
					x = redcap.x,
					spr_x = -redcap.flipped/8,
					y = redcap.y,
					spr_y = -8,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 92,
					x = redcap.x,
					spr_x = redcap.flipped,
					y = redcap.y,
					spr_y = 0,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 93,
					x = redcap.x,
					spr_x = 0,
					y = redcap.y,
					spr_y = 0,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 94,
					x = redcap.x,
					spr_x = -redcap.flipped,
					y = redcap.y,
					spr_y = 0,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 101,
					x = redcap.x,
					spr_x = redcap.flipped,
					y = redcap.y,
					spr_y = 8,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 102,
					x = redcap.x,
					spr_x = 0,
					y = redcap.y,
					spr_y = 8,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 103,
					x = redcap.x,
					spr_x = -redcap.flipped,
					y = redcap.y,
					spr_y = 8,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 115,
					x = redcap.x,
					spr_x = redcap.flipped*7/8,
					y = redcap.y,
					spr_y = 16,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 116,
					x = redcap.x,
					spr_x = -redcap.flipped/8,
					y = redcap.y,
					spr_y = 16,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 124,
					x = redcap.x,
					spr_x = redcap.flipped*3/4,
					y = redcap.y,
					spr_y = 24,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 125,
					x = redcap.x,
					spr_x = -redcap.flipped/4,
					y = redcap.y,
					spr_y = 24,
					facing = redcap.facing_flip,
					spr_flip_v = false
				}
			})

		redcap_windup = {}
			add(redcap_windup,{
				{
					sprite = 87,
					x = redcap.x,
					spr_x = redcap.flipped,
					y = redcap.y,
					spr_y = -8,
					facing = redcap.facing_flip,
					spr_flip_v = false,
				},
				{
					sprite = 88,
					x = redcap.x,
					spr_x = 0,
					y = redcap.y,
					spr_y = -8,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 95,
					x = redcap.x,
					spr_x = redcap.flipped*3/2,
					y = redcap.y,
					spr_y = 0,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 96,
					x = redcap.x,
					spr_x = redcap.flipped/2,
					y = redcap.y,
					spr_y = 0,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 97,
					x = redcap.x,
					spr_x = -redcap.flipped/2,
					y = redcap.y,
					spr_y = 0,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 107,
					x = redcap.x,
					spr_x = redcap.flipped*3/2,
					y = redcap.y,
					spr_y = 8,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 108,
					x = redcap.x,
					spr_x = redcap.flipped/2,
					y = redcap.y,
					spr_y = 8,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 109,
					x = redcap.x,
					spr_x = -redcap.flipped/2,
					y = redcap.y,
					spr_y = 8,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 119,
					x = redcap.x,
					spr_x = redcap.flipped*5/8,
					y = redcap.y,
					spr_y = 16,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 120,
					x = redcap.x,
					spr_x = -redcap.flipped*3/8,
					y = redcap.y,
					spr_y = 16,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 124,
					x = redcap.x,
					spr_x = redcap.flipped/2,
					y = redcap.y,
					spr_y = 24,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 125,
					x = redcap.x,
					spr_x = -redcap.flipped/2,
					y = redcap.y,
					spr_y = 24,
					facing = redcap.facing_flip,
					spr_flip_v = false
				}
			})
			add(redcap_windup,{
				{
					sprite = 87,
					x = redcap.x,
					spr_x = redcap.flipped*7/8,
					y = redcap.y,
					spr_y = -8,
					facing = redcap.facing_flip,
					spr_flip_v = false,
				},
				{
					sprite = 88,
					x = redcap.x,
					spr_x = -redcap.flipped/8,
					y = redcap.y,
					spr_y = -8,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 92,
					x = redcap.x,
					spr_x = redcap.flipped,
					y = redcap.y,
					spr_y = 0,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 93,
					x = redcap.x,
					spr_x = 0,
					y = redcap.y,
					spr_y = 0,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 94,
					x = redcap.x,
					spr_x = -redcap.flipped,
					y = redcap.y,
					spr_y = 0,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 106,
					x = redcap.x,
					spr_x = redcap.flipped,
					y = redcap.y,
					spr_y = 8,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 105,
					x = redcap.x,
					spr_x = 0,
					y = redcap.y,
					spr_y = 8,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 119,
					x = redcap.x,
					spr_x = redcap.flipped,
					y = redcap.y,
					spr_y = 16,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 120,
					x = redcap.x,
					spr_x = 0,
					y = redcap.y,
					spr_y = 16,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 124,
					x = redcap.x,
					spr_x = redcap.flipped*7/8,
					y = redcap.y,
					spr_y = 24,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 125,
					x = redcap.x,
					spr_x = -redcap.flipped/8,
					y = redcap.y,
					spr_y = 24,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 85,
					x = redcap.x,
					spr_x = redcap.flipped*5/8,
					y = redcap.y,
					spr_y = 6,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 86,
					x = redcap.x,
					spr_x = -redcap.flipped*3/8,
					y = redcap.y,
					spr_y = 6,
					facing = redcap.facing_flip,
					spr_flip_v = false
				}
			})
			add(redcap_windup,{
				{
					sprite = 87,
					x = redcap.x,
					spr_x = redcap.flipped*7/8,
					y = redcap.y,
					spr_y = -8,
					facing = redcap.facing_flip,
					spr_flip_v = false,
				},
				{
					sprite = 88,
					x = redcap.x,
					spr_x = -redcap.flipped/8,
					y = redcap.y,
					spr_y = -8,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 92,
					x = redcap.x,
					spr_x = redcap.flipped,
					y = redcap.y,
					spr_y = 0,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 93,
					x = redcap.x,
					spr_x = 0,
					y = redcap.y,
					spr_y = 0,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 94,
					x = redcap.x,
					spr_x = -redcap.flipped,
					y = redcap.y,
					spr_y = 0,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 104,
					x = redcap.x,
					spr_x = redcap.flipped,
					y = redcap.y,
					spr_y = 8,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 105,
					x = redcap.x,
					spr_x = 0,
					y = redcap.y,
					spr_y = 8,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 117,
					x = redcap.x,
					spr_x = redcap.flipped,
					y = redcap.y,
					spr_y = 16,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 118,
					x = redcap.x,
					spr_x = 0,
					y = redcap.y,
					spr_y = 16,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 122,
					x = redcap.x,
					spr_x = redcap.flipped*7/8,
					y = redcap.y,
					spr_y = 24,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 123,
					x = redcap.x,
					spr_x = -redcap.flipped/8,
					y = redcap.y,
					spr_y = 24,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 84,
					x = redcap.x,
					spr_x = -redcap.flipped*9/8,
					y = redcap.y,
					spr_y = 24,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 85,
					x = redcap.x,
					spr_x = redcap.flipped/2,
					y = redcap.y,
					spr_y = 5,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 86,
					x = redcap.x,
					spr_x = -redcap.flipped/2,
					y = redcap.y,
					spr_y = 5,
					facing = redcap.facing_flip,
					spr_flip_v = false
				}
			})
			add(redcap_windup,{
				{
					sprite = 87,
					x = redcap.x,
					spr_x = redcap.flipped*7/8,
					y = redcap.y,
					spr_y = -8,
					facing = redcap.facing_flip,
					spr_flip_v = false,
				},
				{
					sprite = 88,
					x = redcap.x,
					spr_x = -redcap.flipped/8,
					y = redcap.y,
					spr_y = -8,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 92,
					x = redcap.x,
					spr_x = redcap.flipped,
					y = redcap.y,
					spr_y = 0,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 93,
					x = redcap.x,
					spr_x = 0,
					y = redcap.y,
					spr_y = 0,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 94,
					x = redcap.x,
					spr_x = -redcap.flipped,
					y = redcap.y,
					spr_y = 0,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 104,
					x = redcap.x,
					spr_x = redcap.flipped,
					y = redcap.y,
					spr_y = 8,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 105,
					x = redcap.x,
					spr_x = 0,
					y = redcap.y,
					spr_y = 8,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 117,
					x = redcap.x,
					spr_x = redcap.flipped,
					y = redcap.y,
					spr_y = 16,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 118,
					x = redcap.x,
					spr_x = 0,
					y = redcap.y,
					spr_y = 16,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 122,
					x = redcap.x,
					spr_x = redcap.flipped*7/8,
					y = redcap.y,
					spr_y = 24,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 123,
					x = redcap.x,
					spr_x = -redcap.flipped/8,
					y = redcap.y,
					spr_y = 24,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 84,
					x = redcap.x,
					spr_x = -redcap.flipped*9/8,
					y = redcap.y,
					spr_y = 24,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 85,
					x = redcap.x,
					spr_x = 0,
					y = redcap.y,
					spr_y = 5,
					facing = redcap.facing_flip,
					spr_flip_v = false
				},
				{
					sprite = 86,
					x = redcap.x,
					spr_x = -redcap.flipped,
					y = redcap.y,
					spr_y = 5,
					facing = redcap.facing_flip,
					spr_flip_v = false
				}
			})

		redcap_stun = {
			{
				sprite = 89,
				x = redcap.x,
				spr_x = redcap.flipped,
				y = redcap.y,
				spr_y = -8,
				facing = redcap.facing_flip,
				spr_flip_v = false,
			},
			{
				sprite = 90,
				x = redcap.x,
				spr_x = 0,
				y = redcap.y,
				spr_y = -8,
				facing = redcap.facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 91,
				x = redcap.x,
				spr_x = -redcap.flipped,
				y = redcap.y,
				spr_y = -8,
				facing = redcap.facing_flip,
				spr_flip_v = false,
			},
			{
				sprite = 98,
				x = redcap.x,
				spr_x = redcap.flipped,
				y = redcap.y,
				spr_y = 0,
				facing = redcap.facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 99,
				x = redcap.x,
				spr_x = 0,
				y = redcap.y,
				spr_y = 0,
				facing = redcap.facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 100,
				x = redcap.x,
				spr_x = -redcap.flipped,
				y = redcap.y,
				spr_y = 0,
				facing = redcap.facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 104,
				x = redcap.x,
				spr_x = redcap.flipped,
				y = redcap.y,
				spr_y = 8,
				facing = redcap.facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 110,
				x = redcap.x,
				spr_x = 0,
				y = redcap.y,
				spr_y = 8,
				facing = redcap.facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 111,
				x = redcap.x,
				spr_x = -redcap.flipped,
				y = redcap.y,
				spr_y = 8,
				facing = redcap.facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 121,
				x = redcap.x,
				spr_x = redcap.flipped,
				y = redcap.y,
				spr_y = 16,
				facing = redcap.facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 118,
				x = redcap.x,
				spr_x = 0,
				y = redcap.y,
				spr_y = 16,
				facing = redcap.facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 112,
				x = redcap.x,
				spr_x = -redcap.flipped,
				y = redcap.y,
				spr_y = 16,
				facing = redcap.facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 126,
				x = redcap.x,
				spr_x = redcap.flipped,
				y = redcap.y,
				spr_y = 24,
				facing = redcap.facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 127,
				x = redcap.x,
				spr_x = 0,
				y = redcap.y,
				spr_y = 24,
				facing = redcap.facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 84,
				x = redcap.x,
				spr_x = -redcap.flipped,
				y = redcap.y,
				spr_y = 24,
				facing = redcap.facing_flip,
				spr_flip_v = false
			}
		}

		if redcap.state == "moving" then
			redcap.frame = redcap_walk[anim_frame]
		elseif redcap.state == "punching" then
			redcap.frame = redcap_windup[anim_frame]
		elseif redcap.state == "stunned" then
			redcap.frame = redcap_stun
		end
		for tile in all(redcap.frame) do
			add(sprites, tile)
		end
	end--]]
--[[ sort sprites
	sorted_sprites = {}
	for sprite in all(sprites) do
		for key = 0, 128 do
			if ceil(sprite.y) == key then
				add(sorted_sprites,sprite)
			end
		end
	end--]]

end
end
function _draw()
	cls()
	if lives != 0 then
	map(0, 0, 0, 0, 16, 16)
	for sprite in all(sprites) do
		spr(sprite.sprite, sprite.x+sprite.spr_x, sprite.y+sprite.spr_y, 1, 1, sprite.facing, sprite.spr_flip_v)
	end
--[[debug
	print(palette,0,100)
	-- [[
	print(bin)
	-- [[
	print(ceil(note/rate)) --current beat --]] 
	
-- track
	spr(168,note_x+8,120)
	spr(168,note_x+16,120)
	spr(168,note_x+24,120)
	spr(168,note_x+32,120)
	spr(168,note_x+40,120)
	spr(168,note_x+48,120)
	spr(168,note_x+56,120)
	spr(168,note_x+64,120)
	spr(168,note_x+72,120)
	spr(168,note_x+80,120,-(note_x/8),1)
	spr(156,88,120-flr(anim_frame%2),4*(lives/30),1)
	spr(beat,0,120)
	spr(x,64,100)
	spr(z,54,100)
	else
		if display_score < score then display_score += 1 end
		for i=1, display_score do
			x = i%21
			spr(87,x*7-6,(flr(i/21)*7)+x/3,2,1)
		end
		for sprite in all(clover_state) do
			spr(sprite.sprite, sprite.x+sprite.spr_x, sprite.y+sprite.spr_y, 1, 1, sprite.facing, sprite.spr_flip_v)
		end
		spr(155+(flr(time()%2))*6,60,116)
	end
end
__gfx__
00000000000000000556766554500000000000000000000000000000000000000004000054500000222222222222222733333333333333360040004000400000
0000000000000000055676655450005fffffffffffffff01111111111111110f0004000054500000222222222222222733333333333333360000000400040000
0000000000000000055676655450000ffffffffffffffff011111111111111104000400054500000222222222222222733333333333333364000400440000000
00000000000000000556766554500005fffffffffffffff511111111111111190400040054500000222222222222222733333333333333360400440404000000
00000000000000000556766554500000222222222222222fbbbbbbbbbbbbbbb100400040545000002222222222222227333333333333333604404044000000bb
00000000000000000556766554500000fffffffffffffff7111111111111111b00400004545000002222222222222227333333333333333604044004000400ff
000000004000000005567665545000002222222222222226bbbbbbbbbbbbbbb640404000545000002222222222222227333333333333333644004010400000ff
000000000404000005567665545000002222222222222227333333333333333604400400545000005555555555555556111111111111111f04000011000000f9
0556766554500000000000000000000000000000000000000000000000400011104000f9afffffafff9000400000000055500510001111001000000000001100
0556766554500000000000000000000000000000000000000000000000040010110400f9aff00faf9f90004065555550555005100bbbb11b1111111111101100
0556766554500000000000000000000000000000000000000000000040044011011000f9af800fafaf90404065555550555005101bbb11311111111111101100
05567665545000000000000000000ff000000000000000000000ff0004040011101000f9af008fafaf900440766666505550051013311311111111b111101101
bbbbbbaaa950000000000000fff0051000000011110010000000110000440010111000f9af00afafff900040766666505550051013bb3bbbbbbbb1b111101101
ffffffaaa9500000655555505550051000000111100100000000110000040011011000f9af00afa99990000076666650555005101bb3bbbbbbbbb1b111101101
ffffffaaa9504000655555505550051000001111001000000000110000004011111000f9af00afaaa950400076666650555005100b3bbbbbbbbbb1b111101101
aaaa9faaa9500400655555005550051000011110010000000000110004000401101000f9af008faaa9500400766666505550051003bbbbbbbbbbb1b111101100
1000000000400440111000f9af800faaa9500440766666501bbbbbbbbbbbb1b1111011001bbbbbbbbbbbb1b1bb1b111000988840404000000ff2333554504040
1111111000040404011400f9afa00faaa9500400766666500bbbbbbbbbbbb1b1111011010bbbbbbbbbbbb1b1bb1b111000098884400400000ff2333554504000
1111111040044400401000f9affaafaa94504400766666500bbbbbbbbbbbb1b1111011000bbbbbbbbbbbb1b1bb1b111000009999400000000bb3333554504000
111b111004040400040000fbbbbbffa954500400766666500bbbbbbbbbbbb1b1111011000bbbbbbbbbbbb1b1bb1b11100000008884e004000bb3333554500400
bb1b111008440040004000899999999554500040766666500bbbbbbbbbbbb1b1111011000bbbb3333333b1b1bb1b1110000000000cd000000111333554500040
bb1b111009940004004400000556766554500000766666500bbbbbbbbbbbb1b1111011000bbbb3111133b1b1bb1b1110000000000ddc00000bb3333554500000
bb1b111008884000404040000556333554504000766666500bbbbbbbbbbbb1b1111011000bbbb3333333b1b1bb1b1110000000000dddc0000b11333554504000
bb1b111000888400400004000bb3333554504400766666500bbbbbbbbbbbb1b1111011000bbbb3131113b1b1bb1b1110000000000deddce00bb3366554500400
0bbbbbbbbbbbb10000101100100003333333b1b1111011000bbbbbbbbb1b11100000000000ddddd005567665545000000bbbbbbb0bb3111313bbb1b100000000
0bbbb3333333b10110101100101103131113b1b1111011000bbbbbbbbb1b11100000000000ddeddc05567665545000000bbbbbbb0bb3333333bbb1b105555555
0bbbb3113113b10100101100101003333333b1b1111011000bbbbbbbbb1b111000000000000dddc00556741fffff00000bbbbbbb0bb3131133bbb1b105555335
0bbbb3333333b10100101100101003131113b1b1111011000111111111111110b0000000000000000556741f4bbbbbb00bbbbbbb0bb3333333bbb1b105553335
0bbbb3111313b10000101100100003333333b1b1111011000000000000000000bb000000000000000556741f411400000bbbbbbb0bb3113113bbb1b105663336
0bbbb3333333b1000010110010000bbbbbbbb1b1111011007777777777777776bbb00000000000000556741f41ff00000bbbbbbb0bb3333333bbb1b1056666cd
0bbbb3131133b1000010110010000bbbbbbbb1b1111011007777777777777766bbbb0000000000000556741f401f00000bbbbbbb0bbbbbbbbbbbb1b10566666d
0bbbb3333333b1b1111011000bb3333333bbb1b1111011006666666666666660bbbbb000000000000556741f41ff00000bbbbbbb0bbbbbbbbbbbb1b10444445d
00000000bbbbbb000556741f41ff0000555005100bbbbbbbbbbbb1b1111011000000644d11111111000000000000000000001113bbbbbbbbbbbbbb1b00000000
55555554bbbbbbb00556741f401f0000555005100111111111111111111011000006444c3bbbbbbbbbbbbbb1bbbbbbbb00000111333333333333333100000000
55555564bbbbbbbb0556744ffbbb0000555005000000000000000000000000000006654413bbbbbbbbbbbbbb1bbbbbbb00000011111111111111111100000000
53356664bbbbbbbb05567644444400005550000000fcccccccccccccccf0000000056666113bbbbbbbbbbbbbb1bbbbbb00000001111111111111111100000000
37736664bbbbbbbb055676655450000055000000000fcccccccccccccccf0000000055661113bbbbbbbbbbbbbb1bbbbb00000000000000000000000000000000
33736664bbbbbbbb4456766554440000500000000000fcccccccccccccccf0000000555501113bbbbbbbbbbbbbb1bbbb00000000000000000000000033333333
c3366664bbbbbbbb04446555444000000000000000000fcccccccccccccccf0000000555001113bbbbbbbbbbbbbb1bbb00000000000000000000000003333333
d5444444bbbbbbbb000444444000000000000000000000000000000000000000000005550001113bbbbbbbbbbbbbb1bb00000000000000000000000000333333
000000000000000000000000000000000000000000ccccccccccccc00000000444440000000000000444440000000000044444ee4444eeeeec00000000004444
00000000000000000000000000000000400000000ceeedcdeeeeddcc0000004222224000000000004222224000000000000438e4833eeddec000000000000043
0000000000133333333333333133333340000000ceeeeeeeeeeeeedc00000422222224000000000422222224000000000ccc38e8833eee64000000000000ccc3
000000000113bbbbbbbbbbbbbb1bbbbb40000000ceeeeeeeeeeeeeec00004333332222400000004222222222400000000ceeeeeeeeeee640000000000000ceee
0000000001113bbbbbbbbbbbbbb1bbbb40000000ceeeeeceeeeeeeec000443333322224000000442222222224000000000cceeeeeeedc4000000000000000cce
33333300001113bbbbbbbbbbbbbb1bbb00000000ceeeeecdeeeeeedc0042222222222240000054733332222264cccc000000ceeeeddcb444400000000000000c
333333300001113bbbbbbbbbbbbbb1bb00000000cceeecccdeeeedc004255552222226400cccc433333722226deedc0000004ccccccdb3333400000000ccccc0
3333333300001113bbbbbbbbbbbbbb1b000000000cccccc0cccccc0042545ddd226666cc00cd422222222226deecc0000004bbb3dddd3333334000000ceeedcc
4ee4444eeeeec000000422222222566deec00000004bbbb3ddde3333334000000004bbb3ddde33330004bbb3ceeeeeeeeeeeeb3333340000ced444bbbb340000
8e4833eeddec00000004222222254deeedc00000004bbb33eee333bbbb400000004bbb33eee33333004bbb33ceeeeeeeeeeeeb3333340000cecbbbb333bc0000
8e8833eee6400000000142222254b33eec000000004ddc33333331dddd40000000cccc3333333333004bbb33ceeeeedeeeeeeb3333340000cecbb31bddec0000
eeeeeeee6400000000013444444b333bdc000000004ccc33333331cdee4000000ceeec33bbbbbbb40cccbbbbceeeeecdeeeddb33333b4000cc3b33bcdeed4000
eeeeeedc400000000001338bddd3833bc000000000ceeec333ceeeecee4000000ceedd13bbbbbb400cddcbbbceeeedccddddcc1333b34000333b33bceeeec000
eeeedd10000000000000133eeeee33bd4000000000ceeeec3cedeeecee4000000ceeee13bbbbbb400cedcbbb0cdddccccccccbbbb3334000333333bceeeec000
ccccc11114000000000001ceeeeeedc11400000000ceeeecbceeeeecee4000000ceeed133bbbb4000ceecbbb00cccc004bbbbbb33333340033333b40ceeddc00
eeeedb3333400000000044bceeed441bbb40000000ceeeccbbcdeeece400000004ceee13333334000ccc333300000000433333333333340033333b40cdeeed40
ceeeeed40ceeec666ceeeec40ceeec666ceeeec400444466666664400046666666666440004444660008aa99899aa8b4008a883b8aaaaa80000899888a9998b4
4edeeee404ccc66666cccc8004ccc66666cccc8000466666666664800046666666666480004666660008aaa998a88b5500083b448aaaaa800008885548988b35
4ecdeec0008aaaa988a9aa80008aa998aaa9aa80008aaaa988a9aa80008aa998aaa9aa80008aaaa9000888888883355504455555888888800045555554853355
0c4dec00008aaaa98aaa9a8008aaa998aaaa9980008aaaa98aaa9a8008aaa998aaaa998008aaaa990000433b4455555645555555443333400455556654555555
004c4000008aaa98aaaa88008aaaaa998aaaaa80008aaa98aaaa88008aaaaa998aaaaa808aaaa998004455545555556445555554455555540455666646555556
00000000008aaa8aaaaaa8008aaaaaa98aaaaa80008aaa8aaaaaa8008aaaaaa98aaaaa8088aaa98a045555445555664446666445555555540456666646555564
000000000008aa8aaaaaaa8008a889a98aaaaa800008aa8aaaaaaa8008a889a98aaaaa80089a998a455555446666440004444455555555540046664444666640
000000000008aa98a889aa8000899aa88aaaaaa80008aa98a889aa8000899aa88aaaaaa800899998466666654444400000000466666666640004440004444400
00ffff0000ffff0000ffff0000ffff00000000000000000000000000000000000000000088888000000000000000000000000000000000000000000000000000
0ffffff00ffffff00fff5ff00ffffff0000000000000000000000000008888000000088833333800000000000444000000000000000000000000440004404800
ffff33ffffff39ffffff35fffff55fff0003300000000000000000008833338000008b33333333800000444004ff400000000444400000000004ff404ff4f380
ffff39ffffff39ffffff39ffffff55ff0033330000333300003330008333333800008b333bb333800004ff4004f34800000004fff4000000004f3f404f3ff380
ffff39ffffff33ffffff39ffffffffff0033330000333300003330008333333800008b333b33338000843f4004fff380000004ff34480000083ff40004ff3800
ffffffffffffffffffffffffffffffff00033000000330000003300083383338000088b333bbbb80083fff4004333380000004fffff380000083ff4000438000
0ffffff00ffffff00ffffff00ffffff00000000000000000000000000883338000000888b3333800083333888888880000888833333388800888338888888880
00ffff0000ffff0000ffff0000ffff00000000000000000000000000000888000000000088888000008888888888000000008888888880000008888888888000
0000000000044f380000000000000000000008880004993800000000000000000000000000000000000000000000000000000000000000000000000000000000
08444000004f3f3800000000000000000000833800049938000000000008800000000000000000000033333100310031022f02f0022f02f0022f02f0022f02f0
83fff400004fff38000000000000000000004938000439380000000000888800008888000088800000111311001313112222222f2222222f2222222f2222222f
83f3f400004fff38000000000000000000044938044439384400000008888880088888800888880000003110000131102222222f2222222f2222222f2222222f
83f440000044444000000000000000000049393849939938444400000888888008888880088888000003110000031100222222ff222222ff222222ff222222ff
8338000000000000000000000000000000499938499999384444000000888800008888000088880000311000003131000f222ff00f222ff00f222ff00f222ff0
08888888888888000000088088888800004999384999993844444000000880000008800000088000033333100311131000f2ff0000f2ff0000f2ff0000f2ff00
000888888888000000008338888800000044448044444480444440000000000000000000000000000111111001100110000ff000000ff000000ff000000ff000
00000000000000000004400000000000000000000000000088aaaaaa4433333324f424f400000000000000000000000000000000111011000000000011111100
00444444004400440044140000044000004400000000440088aaaaaa4433333320002000000000001111110000000000000000001110110000000000bbbbbbb1
00411111004144410441114000041000044100000000414088a8888a44344443f000f000000000001111111000000000000000001110110000000000bbbbbbbb
00004410000414100411111000041000441144400444411488a8888a443444430000000040000000111111190ff000000000000011101100b0000000bbbbbbbb
00044100000441000004100004441440411111100411111188a8888a44344443f888a88840000000bbbbbbb101500fff0000000011101100bb000000bbbbbbbb
004410000044140000041000041111100411000000004110aaa8888a33344443f0000000400400001111111b015005550555555611101100bbb00000bbbbbbbb
044144400441414000041000004111000041000000004100aaa88aaa33344333a000000040004000bbbbbbb6015005550555555611101107bbbb0000bbbbbbbb
04111110041004100000000000041000000000000000000088888aaa4444433300000000000004003333333601500555005555561110110611111000bbbbbbbb
d4460000015005550000000005666667000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
c4446000015005550555555605666667000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
44566000005005550555555605666667000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
66665000000005550566666705666667000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
66550000000000550566666705666667000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
55550000000000050566666705555556000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
55500000000000000566666705555556000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
55500000000000000566666700000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
__gff__
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
__map__
0100020304050607040506070405060700000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
08a902090a0b0c0d0a0b0c0d0a0b0c0d00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
0e0f101112131415161415161415131200000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
1718191a1b1c1d1e1f1d1e1f1d201c1b00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
21222324251c262728292a28262b1c2500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
2c2d2e2f251c30313233343536371c2500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
38393a3b251c3c27353d3e353f401c2500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
413842431b4445464745464748b0441b00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
494a4b4a4b4a4b4a4b4a4b4a4b4a4b4a00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
4c4d4e4d4e4d4e4d4e4d4e4d4e4d4e4d00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
0000004f50000000004f50000000004f00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
0000005152535253525352535253525300000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000800002c6200062000600040000300001000010002b0002a0002500024000230001f0001c0001a000180001600014000110000f0000d0000a000080000700006000050000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000200001f650006003900000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000200001f650006003900000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
0102102010050100501005010050100501005010050100501005010050100501005010050100501005010050100501005010050100501005010050100501005010050100501005010050100501005010050100500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000c000032c4032c4000000000002ec402ec4000000000002bc402bc40000000000026c4026c4028c4028c4000000000002ac402ac4000000000002bc402bc4000000000002ac402ac402bc402bc402dc402dc400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000c00002e8300000026830000002b83000000228300000026830000001f830000001f8300000025830000002b8300000025830000002b83000000258300000028830000001f8300000028830000001f830000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000c0000000002bc300000022c300000026c30000001fc300000022c30000001ac300000022c300000028c300000028c30000001fc300000028c30000001fc300000025c30000001cc300000025c30000001cc300000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000c00002ec402ec4000000000002bc402bc40000000000032c4032c4030c4030c402ec402ec402dc402dc402dc402dc402dc402dc402dc402dc402dc402dc401ac401ec4021c4026c401ec4021c4026c402ac400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000c00002b83000000228300000027830000001f830000002b8300000022830000002b8300000022830000002a83000000218300000021830000001a8300000012830000001a830000001a8300000021830000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000c00000000027c30000001fc300000022c30000001bc300000027c30000001fc300000027c30000001fc300000026c30000001ec30000001ec300000015c300000015c30000001ec30000001ec300000024c300000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000c00002bc402bc4000000000002dc402dc4000000000002ec402ec40000000000026c4026c4028c4028c4000000000002ac402ac4000000000002bc402bc4000000000002ac402ac402bc402bc402dc402dc400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000c000026830000001f8300000026830000001f830000002b83000000228300000022830000001c8300000024830000001c8300000028830000001f8300000018830000001f830000001c8300000024830000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000c00000000022c30000001ac300000022c30000001ac300000026c30000001fc30000001fc300000018c30000001fc300000018c300000024c30000001cc30000001cc300000024c30000001fc300000028c300000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000c00002ec402ec4000000000002bc402bc40000000000032c4032c402ec402ec402bc402bc402dc402dc402dc402dc402dc402dc402dc402dc402dc402dc402dc4026c402ac402dc4032c402ac402dc4032c400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000c00002b8300000025830000002883000000228300000025830000001f8300000019830000001e83000000268300000021830000002a8300000024830000002a8300000026830000002d830000002a830000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000c00000000028c300000022c300000025c30000001fc300000022c30000001cc30000001ac300000021c30000001ec300000026c300000021c300000026c300000021c30000002ac300000026c30000002dc300000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
__sfx__
000300003c0603906035060320502e0502c050290502605023050200501d0501a050180501605013050110500f0500c0500a05009050070500505004050040500305003050020500105001050010500105001050
000800002c6200062000600040000300001000010002b0002a0002500024000230001f0001c0001a000180001600014000110000f0000d0000a00008000070000600005000000000000000000000000000000000
000200000060000600006000000000000000002462000000000000000000000000000000000600000000000000600000000000000000000000000000000000000000000000000000000000000000000060024600
0002000000000006000705007050070500705007050070500705004000000001b0000705007050070500705007050070500705007050070000700007000070000700007000000000000000000000000000000000
010210201005010050100501005010050100501005010050100501005010050100501005010050100501005010050100501005010050100501005010050100501005010050100501005010050100501005010050
000c000032c4032c4000000000002ec402ec4000000000002bc402bc40000000000026c4026c4028c4028c4000000000002ac402ac4000000000002bc402bc4000000000002ac402ac402bc402bc402dc402dc40
000c00002e8300000026830000002b83000000228300000026830000001f830000001f8300000025830000002b8300000025830000002b83000000258300000028830000001f8300000028830000001f83000000
000c0000000002bc300000022c300000026c30000001fc300000022c30000001ac300000022c300000028c300000028c30000001fc300000028c30000001fc300000025c30000001cc300000025c30000001cc30
000c00002ec402ec4000000000002bc402bc40000000000032c4032c4030c4030c402ec402ec402dc402dc402dc402dc402dc402dc402dc402dc402dc402dc401ac401ec4021c4026c401ec4021c4026c402ac40
000c00002b83000000228300000027830000001f830000002b8300000022830000002b8300000022830000002a83000000218300000021830000001a8300000012830000001a830000001a830000002183000000
000c00000000027c30000001fc300000022c30000001bc300000027c30000001fc300000027c30000001fc300000026c30000001ec30000001ec300000015c300000015c30000001ec30000001ec300000024c30
000c00002bc402bc4000000000002dc402dc4000000000002ec402ec40000000000026c4026c4028c4028c4000000000002ac402ac4000000000002bc402bc4000000000002ac402ac402bc402bc402dc402dc40
000c000026830000001f8300000026830000001f830000002b83000000228300000022830000001c8300000024830000001c8300000028830000001f8300000018830000001f830000001c830000002483000000
000c00000000022c30000001ac300000022c30000001ac300000026c30000001fc30000001fc300000018c30000001fc300000018c300000024c30000001cc30000001cc300000024c30000001fc300000028c30
000c00002ec402ec4000000000002bc402bc40000000000032c4032c402ec402ec402bc402bc402dc402dc402dc402dc402dc402dc402dc402dc402dc402dc402dc4026c402ac402dc4032c402ac402dc4032c40
000c00002b8300000025830000002883000000228300000025830000001f8300000019830000001e83000000268300000021830000002a8300000024830000002a8300000026830000002d830000002a83000000
000c00000000028c300000022c300000025c30000001fc300000022c30000001cc30000001ac300000021c30000001ec300000026c300000021c300000026c300000021c30000002ac300000026c30000002dc30
000c000033c4033c40000000000032c4032c40000000000030c4030c4000000000002ec402ec402dc402dc4000000000002bc402bc4000000000002ac402ac40000000000027c4027c4026c4026c4024c4024c40
000c000030830000002a8300000024830000002d83000000278300000021830000002a830000002483000000128300000018830000001e8300000015830000001b83000000218300000027830000002d83000000
000c0000000002dc300000027c300000021c30000002ac300000024c30000001ec300000027c300000021c300000015c30000001bc300000021c300000018c30000001ec300000024c30000002ac300000030c30
000c000033c4033c40000000000032c4032c40000000000030c4030c4000000000002ec402ec402dc402dc4000000000002bc402bc4000000000002ac402ac402ac402ac402ac402ac402ac402ac400000000000
000c00002b8300000024830000001b8300000027830000001f83000000188300000024830000001e8300000018830000001e8300000024830000001b83000000218300000027830000002d830000002783000000
000c00000000027c30000001fc300000018c300000024c30000001bc300000013c300000021c30000001bc30000001bc300000021c300000027c30000001ec300000024c30000002ac30000002ac300000024c30
000c000033c4033c40000000000032c4032c40000000000030c4030c4000000000002ec402ec4035c4035c40000000000033c4033c40000000000032c4032c40000000000030c4030c40000000000030c4030c40
000c00003083000000278300000020830000002c8300000024830000001b83000000298300000021830000003083000000298300000021830000002d8300000024830000001d8300000029830000002183000000
000c0000000002cc300000024c30000001bc300000027c300000020c300000018c300000024c30000001dc30000002dc300000024c30000001dc300000029c300000021c300000018c300000024c30000001dc30
000c000036c3036c3036c300000033c4033c40000000000033c4033c4032c4032c4031c4031c4032c4032c4032c4032c4032c4032c4032c4032c4032c4032c401ec4021c4026c402ac4026c402ac402dc4030c40
000c010035c4001400014000140001400014000140001400014000140001400014000140001400014000140001400014000140001400014000140001400014000140001400014000140001400014000140001400
000c1f0033c30000002dc300000030c30000002ac30000002dc300000027c30000002ac300000026c300000026c30000001ec30000002ac300000021c30000001ac300000021c300000021c30000002ac3001400
000c00000000030c30000002ac30000002dc300000027c30000002ac300000024c300000027c300000025c300000021c30000001ac300000026c30000001ec30000001ec300000026c300000026c30000002dc30
000c000426c402ac402dc4030c4022600226002260022600226002260022600226002260022600226002260022600226002260022600226002260022600226002260022600226002260022600226002260000000
001100000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
__music__
01 05464647
00 0849494a
00 0b4c4c4d
00 0e4f4f50
00 11525253
00 14555556
00 17585859
02 1a5b5c5d
00 5e404040
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
00 00000000
