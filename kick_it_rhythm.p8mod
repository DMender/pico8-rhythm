pico-8 cartridge // http://www.pico-8.com
version 42
__lua__
--variables
note = stat(56)
active = false
window_end = 10
window_start = 0
latency = 10
adj_latency = 0
adjust_hits = 0
player = {
	x = 60,
	y = 80,
	speed = 0.3,
	flipped = 4
}
punch_flip = false
kick_flip = false
facing_flip = false
note_x = 0
beat = 166
synco = true
anim_frame = 1
x = 155
z = 154
once_punch = false
once_kick = false
rate = 24
once_beat = false
once_anim = true
spawn = {
{facing_flip = true,
	x = 128,
	y = 70,
	speed = 0.15,
	state = moving,
	flipped = 8,
	life = 3
	},
	{facing_flip = true,
	x = 0,
	y = 70,
	speed = 0.15,
	state = moving,
	flipped = 8,
	life = 3
	},
	{facing_flip = true,
	x = 144,
	y = 86,
	speed = 0.15,
	state = moving,
	flipped = 8,
	life = 3
	},
	{facing_flip = true,
	x = -16,
	y = 86,
	speed = 0.15,
	state = moving,
	flipped = 8,
	life = 3
	}
}
reserved_right = false
reserved_left = false
right_queue = 0
left_queue = 0
last_beat = 0
tick = 0
once_tick = true
sprites = {}
sorted_sprites = {}
whiff_left = true
whiff_right = true

--functions
function distance(x1, y1, x2, y2)
	local dx = x1 - x2
	local dy = y1 - y2
	return flr(sqrt(dx*dx + dy*dy))
end

function _init()
	music()
	pal({[0]=0,5,8,7,128,132,4,143,129,1,140,6,131,3,139,15},1)
	poke(0x5f5f,0x3f)
	pal({[0]=136,133,133,12,12,136,134,134,14,14,14,14,14,14,136,12},2)
end

function _update60()
	tick += 1
	if #spawn < 4 then
		redcap = {}
		if tick%2 == 0 then
		redcap = {facing_flip = true,
		x = -16,
		y = 86,
		speed = 0.15,
		state = moving,
		flipped = 8,
		life = 3
		}
		else
		redcap = {facing_flip = true,
		x = 128,
		y = 86,
		speed = 0.15,
		state = moving,
		flipped = 8,
		life = 3
		}
		end
		add(spawn,redcap)
	end
	note = stat(56)
	-- movement normalization
	norm = 1
	if (btn(⬆️) or btn(⬇️)) and (btn(⬅️) or btn(➡️)) then
		norm = 1
	else
		norm = 1.4
	end

-- clover states
	clover_idle = {
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
			sprite = 152, --hand shadow
			x = player.x,
			spr_x = player.flipped,
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
			sprite = 133, --hand
			x = player.x,
			spr_x = player.flipped,
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
			spr_y = 10,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 138, --shoe
			x = player.x,
			spr_x = -player.flipped,
			y = player.y,
			spr_y = 10,
			facing = facing_flip,
			spr_flip_v = false
		}
	}

	clover_walk = {}
		add(clover_walk,{
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
				sprite = 152, --hand shadow
				x = player.x,
				spr_x = player.flipped,
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
				sprite = 133, --hand
				x = player.x,
				spr_x = player.flipped,
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
				sprite = 141, --shoe
				x = player.x,
				spr_x = player.flipped,
				y = player.y,
				spr_y = 10,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 140, --shoe
				x = player.x,
				spr_x = -player.flipped,
				y = player.y,
				spr_y = 10,
				facing = facing_flip,
				spr_flip_v = false
			}
		})
		add(clover_walk,{
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
				sprite = 152, --hand shadow
				x = player.x,
				spr_x = player.flipped,
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
				sprite = 133, --hand
				x = player.x,
				spr_x = player.flipped,
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
				spr_y = 10,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 142, --shoe
				x = player.x,
				spr_x = -player.flipped,
				y = player.y,
				spr_y = 10,
				facing = facing_flip,
				spr_flip_v = false
			}
		})
		add(clover_walk,{
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
				sprite = 152, --hand shadow
				x = player.x,
				spr_x = player.flipped,
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
				sprite = 133, --hand
				x = player.x,
				spr_x = player.flipped,
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
				sprite = 145, --shoe
				x = player.x,
				spr_x = player.flipped,
				y = player.y,
				spr_y = 10,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 144, --shoe
				x = player.x,
				spr_x = -player.flipped,
				y = player.y,
				spr_y = 10,
				facing = facing_flip,
				spr_flip_v = false
			}
		})
		add(clover_walk,{
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
				sprite = 152, --hand shadow
				x = player.x,
				spr_x = player.flipped,
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
				sprite = 133, --hand
				x = player.x,
				spr_x = player.flipped,
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
				spr_y = 10,
				facing = facing_flip,
				spr_flip_v = false
			},
			{
				sprite = 142, --shoe
				x = player.x,
				spr_x = -player.flipped,
				y = player.y,
				spr_y = 10,
				facing = facing_flip,
				spr_flip_v = false
			}
		})

	clover_kick = {
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
			sprite = 152, --hand shadow
			x = player.x,
			spr_x = player.flipped,
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
			sprite = 133, --hand
			x = player.x,
			spr_x = player.flipped,
			y = player.y,
			spr_y = 2,
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
			spr_y = 10,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 138, --shoe
			x = player.x,
			spr_x = -player.flipped,
			y = player.y,
			spr_y = 10,
			facing = facing_flip,
			spr_flip_v = false
		}
	}

	clover_punch = {
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
			sprite = 152, --hand shadow
			x = player.x,
			spr_x = player.flipped,
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
			sprite = 133, --hand
			x = player.x,
			spr_x = player.flipped,
			y = player.y,
			spr_y = 2,
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
			spr_y = 10,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 138, --shoe
			x = player.x,
			spr_x = -player.flipped,
			y = player.y,
			spr_y = 10,
			facing = facing_flip,
			spr_flip_v = false
		}
	}

	clover_whiff = {
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
			sprite = 152, --hand shadow
			x = player.x,
			spr_x = player.flipped,
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
			sprite = 133, --hand
			x = player.x,
			spr_x = player.flipped,
			y = player.y,
			spr_y = 2,
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
			spr_y = 10,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 138, --shoe
			x = player.x,
			spr_x = -player.flipped,
			y = player.y,
			spr_y = 10,
			facing = facing_flip,
			spr_flip_v = false
		}
	}

	clover_stun = {
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
			sprite = 152, --hand shadow
			x = player.x,
			spr_x = player.flipped,
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
			sprite = 133, --hand
			x = player.x,
			spr_x = player.flipped,
			y = player.y,
			spr_y = 2,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 131, --body
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
			spr_y = 10,
			facing = facing_flip,
			spr_flip_v = false
		},
		{
			sprite = 138, --shoe
			x = player.x,
			spr_x = -player.flipped,
			y = player.y,
			spr_y = 10,
			facing = facing_flip,
			spr_flip_v = false
		}
	}
	
	--all beats
	if flr(note/rate) !=  last_beat then
		last_beat = flr(note/rate)
		--beat track
		note_x -= 1
		if note_x == -8 then
			note_x = 0
		end
	end
	--latency adjustment
	if (flr(note/rate))%2 != 0 then
		if once_tick then
			tick = 0
			once_tick = false
		end
	end
	-- on beats
	if (flr(note/rate))%2 == 0 then
		if once_anim then
			once_tick = true
		--animation
			anim_frame += 1
		clover_state = clover_idle
			if anim_frame == 5 then
				anim_frame = 1
			end
			once_anim = false
		end
		synco = false
		if once_beat == false then
			window_start = tick - latency
			sfx(2)
			once_beat = true
		end
	else
		once_anim = true
		once_beat = false
		once_punch = false
		once_kick = false
		punch_flip = facing_flip
		kick_flip = facing_flip
	end
	if tick >= window_start then
		active = true
	end
	if tick <= window_start or tick >= window_start + window_end then
		active = false
	end
	--off beats
	if (flr(note/rate))%4 == 0 then
		synco = true
	end
	-- up
	if btn(⬆️) and player.y > 65 then
		if adjust_hits <= 12 then
			return
		end
		clover_state = clover_walk[anim_frame]
		player.y -= 1 * player.speed * norm
	-- down
	elseif btn(⬇️) and player.y < 104 then
		if adjust_hits <= 12 then
			return
		end
		clover_state = clover_walk[anim_frame]
		player.y += 1 * player.speed * norm
	end
	-- left
	if btn(⬅️)  and player.x > 0 then
		if adjust_hits <= 12 then
			return
		end
		clover_state = clover_walk[anim_frame]
		player.flipped = -4
		facing_flip = true
		player.x -= 1.7 * player.speed * norm
	-- right
	elseif btn(➡️) and player.x < 112 then
		if adjust_hits <= 12 then
			return
		end
		clover_state = clover_walk[anim_frame]
		player.flipped = 4
		facing_flip = false
		player.x += 1.7 * player.speed * norm
	end
	--punch
	if btnp(❎) and active and not once_punch then
		whiff_left = false
		whiff_right = false
		beat = 167
		sfx(1)
		once_punch = true
		punch_flip = not punch_flip
		clover_state = clover_punch
	elseif btnp(❎) then
		whiff_left = true
		whiff_right = true
		clover_state = clover_whiff
		beat = 166
		once_punch = true
	end
	--kick
	if btnp(🅾️) and active and synco and not once_kick then
		whiff_left = false
		whiff_right = false
		beat = 167
		sfx(1)
		once_kick = true
		kick_flip = not kick_flip
		clover_state = clover_kick
	elseif btnp(🅾️) then
		whiff_left = true
		whiff_right = true
		beat = 166
		clover_state = clover_punch
		once_kick = true
	end

	-- calibration
	if adjust_hits <= 12 then
		if btnp(❎) then
			adjust_hits += 1
			if adjust_hits == 1 then
				--adj_latency = 
			elseif adjust_hits < 12 then
				--adj_latency = 
			end
		end
		if adjust_hits == 12 then
			--latency = adj_latency/2
		end
	end

	--tutorial animations
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
	end
	
	--red caps
	right_queue = 0
	left_queue = 0
	for k,i in pairs(spawn) do
		if i.life == 0 then
			del(spawn, i)
		end
		i.state = punching
		if distance(player.x+8,player.y,i.x,i.y) < 24  and i.x > player.x+8 then
			reserved_right = true
			right_queue += 1
			--TODO: might need a once punch
			if not whiff_right and not facing_flip and (btnp(🅾️) or btnp(❎)) then
				i.life -= 1
				i.state = stunned
				whiff_right = true
			end
		end
		if distance(player.x,player.y,i.x+8,i.y) < 24  and i.x+8 < player.x then
			reserved_left = true
			left_queue += 1
			if not whiff_left and facing_flip and (btnp(🅾️) or btnp(❎)) then
				i.life -= 1
				i.state = stunned
				whiff_left = true
			end
		end
		if i.x - 4 > player.x + 8 and (distance(player.x+8,player.y,i.x,i.y) < 24 or not reserved_right) then
			i.x -= 1.7 * i.speed
			i.facing_flip = false
			i.flipped = -8
			i.state = moving
			
			if i.y > player.y - 16 + k * 4 then
				i.y -= 1 * i.speed
				i.state = moving
			end
			if i.y < player.y - 16 + k * 4 then
				i.y += 1 * i.speed
				i.state = moving
			end
		end
		if i.x + 8  < player.x - 4  and (distance(player.x,player.y,i.x+8,i.y) < 24 or not reserved_left) then
			i.x += 1.7 * i.speed
			i.facing_flip = true
			i.flipped = 8
			i.state = moving
			
			if i.y > player.y - 16 + k * 4 then
				i.y -= 1 * i.speed
				i.state = moving
			end
			if i.y < player.y - 16 + k * 4 then
				i.y += 1 * i.speed
				i.state = moving
			end
		end
		if i.x + 8 > player.x - 4 and i.x + 8 < player.x + 8 then
			i.x -= 1.7 * i.speed
			i.facing_flip = true
			i.flipped = 8
			i.state = moving
		end
		if i.x - 4 < player.x + 16 and i.x >= player.x then
			i.x += 1.7 * i.speed
			i.facing_flip = false
			i.flipped = -8
			i.state = moving
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
		testredcaplocal = {
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
			}
		}
		if redcap.state == moving then
			redcap.state = testredcaplocal
		end
		if redcap.state == punching then
			redcap.state = testredcaplocal
		end
		if redcap.state == stunned then
			redcap.state = testredcaplocal
		end
		for tile in all(redcap.state) do
			add(sprites, tile)
		end
	end
	-- sort sprites
	sorted_sprites = {}
	for sprite in all(sprites) do
		for key = 1, 128 do
			if flr(sprite.y) == key then
				add(sorted_sprites,sprite)
			end
		end
	end

end

function _draw()
	cls()
	map(0, 0, 0, 0, 16, 16)
	for sprite in all(sorted_sprites) do
		spr(sprite.sprite, sprite.x+sprite.spr_x, sprite.y+sprite.spr_y, 1, 1, sprite.facing, sprite.spr_flip_v)
	end
	--[[
	print(ceil(note/rate)) --current beat 
	print(window_start)--]] 
	
	--track
	spr(168,note_x+8,116)
	spr(168,note_x+16,116)
	spr(168,note_x+24,116)
	spr(168,note_x+32,116)
	spr(168,note_x+40,116)
	spr(168,note_x+48,116)
	spr(168,note_x+56,116)
	spr(168,note_x+64,116)
	spr(168,note_x+72,116)
	spr(168,note_x+80,116)
	spr(168,note_x+88,116)
	spr(168,note_x+96,116)
	spr(168,note_x+104,116)
	spr(168,note_x+112,116)
	spr(168,note_x+120,116)
	spr(168,note_x+128,116)
	spr(beat,0,116)
	spr(x,64,100)
	spr(z,54,100)
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
ceeeeed40ceeec666ceeeec40ceeec666ceeeec400444466666664400046666666666440004444660008aa99199aa8b4008a883b8aaaaa80000899888a9998b4
4edeeee404ccc66666cccc8004ccc66666cccc8000466666666664800046666666666480004666660008aaa998a88b5500083b448aaaaa800008885548988b35
4ecdeec0008aaaa988a9aa80008aa998aaa9aa80008aaaa988a9aa80008aa998aaa9aa80008aaaa9000888888883355504455555888888800045555554853355
0c4dec00008aaaa98aaa9a8008aaa998aaaa9980008aaaa98aaa9a8008aaa998aaaa998008aaaa990000433b4455555645555555443333400455556654555555
004c4000008aaa98aaaa88008aaaaa998aaaaa80008aaa98aaaa88008aaaaa998aaaaa808aaaa998004455545555556445555554455555540455666646555556
00000000008aaa8aaaaaa8008aaaaaa98aaaaa80008aaa8aaaaaa8008aaaaaa98aaaaa8088aaa98a045555445555664446666445555555540456666646555564
000000000008aa8aaaaaaa8008a889a98aaaaa800008aa8aaaaaaa8008a889a98aaaaa80089a998a455555446666440004444455555555540046664444666640
000000000008aa98a889aa8000899aa88aaaaaa80008aa98a889aa8000899aa88aaaaaa800899998466666654444400000000466666666640004440004444400
00ffff0000ffff0000ffff0000ffff00000000000000000000000000000000000000000088888000000000000000000000000000000000000000000000000000
0ffffff00ffffff00fff5ff00ffffff0000000000000000000000000008888000000088833333800000000000444000000000000000000000000440004404800
ffff33ffffff39ffffff35fffff55fff0003300000000000000000008833338000008b3333333380000044400488400000000444400000000004884048848380
ffff39ffffff39ffffff39ffffff55ff0033330000333300003330008333333800008b333bb33380000488400483480000000488840000000048384048388380
ffff39ffffff33ffffff39ffffffffff0033330000333300003330008333333800008b333b333380008438400488838000000488344800000838840004883800
ffffffffffffffffffffffffffffffff00033000000330000003300083383338000088b333bbbb80083888400433338000000488888380000083884000438000
0ffffff00ffffff00ffffff00ffffff00000000000000000000000000883338000000888b3333800083333888888880000888833333388800888338888888880
00ffff0000ffff0000ffff0000ffff00000000000000000000000000000888000000000088888000008888888888000000008888888880000008888888888000
00000000000448380000000000000000000008880004883800000000000000000000000000000000000000000000000000031000000000000000000000000000
08444000004838380000000000000000000083380004883800000000000880000000000000000000003333310031003100333100000310000031000000003100
83888400004888380000000000000000000048380004383800000000008888000088880000888000001113110013131103333310000310000331000000003310
83838400004888380000000000000000000448380444383844000000088888800888888008888800000031100001311001131110000310003333331003333331
83844000004444400000000000000000004838384883883844440000088888800888888008888800000311000003110000031000033333101331111001113311
83380000000000000000000000000000004888384888883844440000008888000088880000888800003110000031310000031000013331100131000000003110
08888888888888000000088088888800004888384888883844444000000880000008800000088000033333100311131000011000001311000011000000001100
00088888888800000000833888880000004444804444448044444000000000000000000000000000011111100110011000000000000110000000000000000000
00000000000000000004400000000000000000000000000088aaaaaa44333333cccccccc244424442444444424444444cccccccc000000000000000000000000
00444444004400440044140000044000004400000000440088aaaaaa4433333324242424200020002000000020000000cccccccc000000000000000000000000
00411111004144410441114000041000044100000000414088a8888a443444432c2c2c2cf000f000f0000000f0000000cccccccc000000000000000000000000
00004410000414100411111000041000441144400444411488a8888a44344443fcfcfcfc000000000000000000000000cccccccc000000000000000000000000
00044100000441000004100004441440411111100411111188a8888a44344443ccccccccf8888888f888888888888888cccccccc000000000000000000000000
004410000044140000041000041111100411000000004110aaa8888a33344443f888f888f0000000f000000000000000cccccccc000000000000000000000000
044144400441414000041000004111000041000000004100aaa88aaa33344333fcccfccca0000000a000000000000000cccccccc000000000000000000000000
04111110041004100000000000041000000000000000000088888aaa44444333acccaccc000000000000000000000000cccccccc000000000000000000000000
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
085402090a0b0c0d0a0b0c0d0a0b0c0d00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
0e0f101112131415161415161415131200000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
1718191a1b1c1d1e1f1d1e1f1d201c1b00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
21222324251c262728292a28262b1c2500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
2c2d2e2f251c30313233343536371c2500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
38393a3b251c3c27353d3e353f401c2500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
413842431b444546474546474848441b00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
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
000100000006000060000600000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000800002c6200062000600040000300001000010002b0002a0002500024000230001f0001c0001a000180001600014000110000f0000d0000a00008000070000600005000000000000000000000000000000000
000200001f65000600390000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
000200001f65000600390000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
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
