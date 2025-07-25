desc: TapeEmu - Professional Tape Machine Emulation
version: 1.5
author: citizenkeith - Claude AI
about: tape emulation that includes tape machine choices
      This JSFX released under GPLv3 license

// Hidden sliders using proper JSFX technique (- prefix hides from default UI)
slider1:68<0,178,1>-Input/Saturation (%)
slider2:-10<-50,0,1>-Output (dB)
slider3:-93<-105,-60,1>-Noise (dB)
slider4:-18<-18,12,0.1>-Head Bump (dB)
slider5:0<0,100,1>-Tape Age (%)
slider6:100<0,200,1>-Stereo Width (%)
slider7:0<0,4,1{Studer A800,Ampex ATR102,Sony APR5000,Otari MTR90,Basic Tape}>-Tape Machine
slider8:0<0,100,1>-Wow & Flutter (%)

in_pin:left input
in_pin:right input
out_pin:left output
out_pin:right output

@init
// Mouse interaction variables
slider_to_edit = 0;
last_mouse_cap = 0;
drag_start_y = 0;
last_mouse_y = 0;

// Tape age and wow/flutter variables
age_dropout_counter = 0;
dropout_counter = 0;
age_noise_seed = 12345;

// Wow and flutter variables
wow_phase = 0;
flutter_phase = 0;
// Simple delay buffer for wow/flutter
wf_buffer_l = 131072; // After right channel buffer
wf_buffer_r = 131072 + 1024;
wf_write_pos = 0;
wf_buffer_size = 512;

//head bump
fftsize=1024;
bucketsize = (srate * 0.5 ) / fftsize;
bufpos=bi1=0;
bi2=fftsize*2;
halfsize=fftsize*0.5;
bands = 1;
gain = exp(slider4 * log(10) / 40);
shift = 0;
  
//compression
gain = seekgain = 0;
c = 8.65617025;
dc = exp(-30 * log(10));
gain = seekGain = 0;
t = 0;
b = -exp(-62.83185307 / srate );
a = 1.0 + b;

// Tape machine preset variables
preset_head_bump = -18;
preset_hf_rolloff = 50;
preset_sat_character = 1.0;
preset_wow_flutter = 25;
preset_noise_floor = -93;

// Function to load tape machine presets
function load_tape_preset(preset_num) (
  preset_num == 0 ? ( // Studer A800
    preset_head_bump = -15;
    preset_hf_rolloff = 60;
    preset_sat_character = 0.8;
    preset_wow_flutter = 25;
    preset_noise_floor = -95;
  ) : preset_num == 1 ? ( // Ampex ATR102
    preset_head_bump = -12;
    preset_hf_rolloff = 45;
    preset_sat_character = 1.2;
    preset_wow_flutter = 35;
    preset_noise_floor = -90;
  ) : preset_num == 2 ? ( // Sony APR5000
    preset_head_bump = -20;
    preset_hf_rolloff = 30;
    preset_sat_character = 0.6;
    preset_wow_flutter = 15;
    preset_noise_floor = -98;
  ) : preset_num == 3 ? ( // Otari MTR90
    preset_head_bump = -16;
    preset_hf_rolloff = 55;
    preset_sat_character = 0.9;
    preset_wow_flutter = 20;
    preset_noise_floor = -96;
  ) : ( // Basic Tape
    preset_head_bump = slider4;
    preset_hf_rolloff = 50;
    preset_sat_character = 1.0;
    preset_wow_flutter = 25;
    preset_noise_floor = slider3;
  );
);

// Function to update audio processing variables when sliders change
function update_audio_vars() (
  // Load tape machine preset
  load_tape_preset(slider7);

  vd = exp(slider2 * log(10) / 20);
  vn_base = exp((slider7 == 4 ? slider3 : preset_noise_floor) * log(10) / 20);

  // Apply tape age effects to noise
  age_factor = 1 + (slider5 / 100) * 3; // Up to 4x noise increase with age
  vn = vn_base * age_factor;

  // Stereo width factor
  width_factor = slider6 / 100;

  // Wow & Flutter amount
  wow_flutter_amount = slider8 / 100;

  // Apply tape machine character to saturation
  sat_input = slider1 * preset_sat_character;
  foo=(sat_input/200*$pi);
  bar = sin(sat_input/200*$pi);

  //head bump - use preset or manual setting
  head_bump_db = (slider7 == 4 ? slider4 : preset_head_bump);
  adj = exp(head_bump_db * log(10) / 40);
  bands = 1;
  gain = exp(0 * log(10) / 40);
  shift = (0 < 1 ? 0 : 1);
);

// Initialize audio processing variables
update_audio_vars();

@slider
// Call the update function when sliders change via host/automation
update_audio_vars();

@block
// No VU processing needed for new GUI

@sample
// Apply tape machine character to saturation amount
effective_saturation = slider1 * preset_sat_character;

//harmonic content with machine-specific character
effective_saturation > 0 ? (
  sat_amount = (effective_saturation/200*$pi);
  sat_norm = sin(effective_saturation/200*$pi);
  
  // Add machine-specific non-linearities
  slider7 == 0 ? ( // Studer A800 - even harmonics
    spl0 = min(max( sin(max(min(spl0,1),-1)*sat_amount)/sat_norm + spl0*spl0*0.1 ,-1) ,1);
    spl1 = min(max( sin(max(min(spl1,1),-1)*sat_amount)/sat_norm + spl1*spl1*0.1 ,-1) ,1)
  );
  
  slider7 == 1 ? ( // Ampex ATR102 - odd harmonics + asymmetry
    spl0 = min(max( sin(max(min(spl0,1),-1)*sat_amount)/sat_norm + spl0*spl0*spl0*0.15 ,-1) ,1);
    spl1 = min(max( sin(max(min(spl1,1),-1)*sat_amount)/sat_norm + spl1*spl1*spl1*0.15 ,-1) ,1)
  );
  
  slider7 == 2 ? ( // Sony APR5000 - clean, minimal distortion
    spl0 = min(max( sin(max(min(spl0,1),-1)*sat_amount)/sat_norm + spl0*spl0*0.05 ,-1) ,1);
    spl1 = min(max( sin(max(min(spl1,1),-1)*sat_amount)/sat_norm + spl1*spl1*0.05 ,-1) ,1)
  );
  
  slider7 == 3 ? ( // Otari MTR90 - balanced harmonics
    spl0 = min(max( sin(max(min(spl0,1),-1)*sat_amount)/sat_norm + spl0*spl0*0.08 + spl0*spl0*spl0*0.05 ,-1) ,1);
    spl1 = min(max( sin(max(min(spl1,1),-1)*sat_amount)/sat_norm + spl1*spl1*0.08 + spl1*spl1*spl1*0.05 ,-1) ,1)
  );
  
  slider7 == 4 ? ( // Basic Tape - original formula
    spl0 = min(max( sin(max(min(spl0,1),-1)*foo)/bar ,-1) ,1);
    spl1 = min(max( sin(max(min(spl1,1),-1)*foo)/bar ,-1) ,1)
  );
);

//White noise with machine-specific characteristics
noise=rand(2)-1;

// Add machine-specific noise coloring
slider7 == 0 ? ( // Studer A800 - warm noise
  noise = noise * 0.8 + sin(noise * 3.14159) * 0.2
);

slider7 == 1 ? ( // Ampex ATR102 - grittier noise
  noise = noise + noise*noise*noise*0.3
);

slider7 == 2 ? ( // Sony APR5000 - clean white noise
  noise = noise // Keep original noise unchanged
);

slider7 == 3 ? ( // Otari MTR90 - slightly colored
  noise = noise * 0.9 + sin(noise * 1.57) * 0.1
);

// Tape age effects - made more noticeable
slider5 > 0 ? (
  // Add age-related dropouts (brief signal reductions)
  age_dropout_counter += 1;
  age_dropout_counter > srate/20 ? ( // Check every 1/20 second (more frequent)
    age_dropout_counter = 0;
    // Random dropout probability based on age - increased for audibility
    dropout_chance = slider5 / 100 * 0.01; // Max 1% chance per check (10x more likely)
    rand() < dropout_chance ? (
      dropout_length = floor(rand() * srate * 0.05); // Up to 50ms dropout (longer)
      dropout_counter = dropout_length;
    );
  );
  
  // Apply dropout if active
  dropout_counter > 0 ? (
    dropout_factor = 0.05 + rand() * 0.2; // Reduce to 5-25% volume (more dramatic)
    spl0 *= dropout_factor;
    spl1 *= dropout_factor;
    dropout_counter -= 1;
  );
  
  // Add age-related modulation noise (more noticeable flutter)
  age_mod = sin(age_noise_seed * 0.01) * slider5 / 100 * 0.001; // 10x more modulation
  age_noise_seed += 1;
  noise += age_mod;
);

// Simple Wow and Flutter implementation (ReaFripp style)
wow_flutter_amount > 0 ? (
  // Write current samples to buffer
  wf_buffer_l[wf_write_pos] = spl0;
  wf_buffer_r[wf_write_pos] = spl1;
  
  // WOW: Slow speed variations (0.5 Hz)
  wow_phase += 0.5 / srate;
  wow_phase >= 1 ? wow_phase -= 1;
  
  // FLUTTER: Fast speed variations (12 Hz)
  flutter_phase += 12 / srate;
  flutter_phase >= 1 ? flutter_phase -= 1;
  
  // Calculate modulation
  wow_mod = sin(2 * $pi * wow_phase);
  flutter_mod = sin(2 * $pi * flutter_phase);
  total_modulation = wow_flutter_amount * (wow_mod * 0.0001 + flutter_mod * 0.00005);
  
  // Calculate read position with modulation
  base_delay = 128;
  read_offset = base_delay * (1 + total_modulation);
  read_pos = wf_write_pos - read_offset;
  read_pos < 0 ? read_pos += wf_buffer_size;
  
  // Simple interpolation
  read_pos_int = read_pos | 0;
  read_pos_frac = read_pos - read_pos_int;
  read_pos_next = (read_pos_int + 1) % wf_buffer_size;
  
  // Read with interpolation
  spl0 = wf_buffer_l[read_pos_int] * (1 - read_pos_frac) +
         wf_buffer_l[read_pos_next] * read_pos_frac;
  spl1 = wf_buffer_r[read_pos_int] * (1 - read_pos_frac) +
         wf_buffer_r[read_pos_next] * read_pos_frac;
  
  // Increment write position
  wf_write_pos = (wf_write_pos + 1) % wf_buffer_size;
);

spl0=spl0*vd+noise*vn;
spl1=spl1*vd+noise*vn;

//head bump
dry0=spl0;
dry1=spl1;
wet0=spl0+spl1;

t=bi1+bufpos;
p0=t[0];
t[0]=wet0;

t=bi2+halfsize+bufpos;
p1=t[0];
t[0]=wet0;

wet0 = p0 + p1; // our mdct handles windowing, so we just add

bufpos+=1;

bufpos >= halfsize ? (
  // we hit our FFT size here
  // swap bi1 and bi2
  t=bi1; bi1=bi2; bi2=t;
  // Map time -> frequency domain
  mdct(bi1,fftsize);
  // frequency-shift energy to lower bands
  // memcpy(bi1,bi1+shift,bands);
  i=0;
  loop(bands, bi1[i]=bi1[i*shift+i]; i+=1;);
  // zero out HF bands;
  memset(bi1+bands,0,fftsize-bands);
  imdct(bi1,fftsize);
  bufpos=0;
);

// Use machine-specific head bump
effective_head_bump = (slider7 == 4 ? slider4 : preset_head_bump);
adj = exp(effective_head_bump * log(10) / 40);

spl0 = (dry0 + wet0 * adj) * gain;
spl1 = (dry1 + wet0 * adj) * gain;

// Stereo width processing (Mid/Side)
mid = (spl0 + spl1) * 0.5;
side = (spl0 - spl1) * 0.5;
side *= width_factor;

// Apply subtle machine-specific frequency coloring to stereo image
slider7 == 0 ? ( // Studer A800 - slightly warm mids
  mid = mid * 1.02;
  side = side * 0.98
);

slider7 == 1 ? ( // Ampex ATR102 - enhanced presence
  mid = mid + mid*mid*0.01;
  side = side * 1.05
);

slider7 == 2 ? ( // Sony APR5000 - clean and accurate
  mid = mid // No coloring, keep clean
);

slider7 == 3 ? ( // Otari MTR90 - slight warmth
  mid = mid * 1.01
);

spl0 = mid + side;
spl1 = mid - side;
  
//add machine-specific compression when driven
threshDB = 0; //set to zero. lower to -6 or so for actual comp
thresh = exp(threshDB/c);

// Machine-specific compression characteristics
slider7 == 0 ? ( // Studer A800 - gentle, musical compression
  ratio = 1/2.5;
  attack = exp( threshDB / (8*srate/1000) / c);
  release = exp( threshDB / (150*srate/1000) / c )
);

slider7 == 1 ? ( // Ampex ATR102 - more aggressive
  ratio = 1/1.5;
  attack = exp( threshDB / (3*srate/1000) / c);
  release = exp( threshDB / (80*srate/1000) / c )
);

slider7 == 2 ? ( // Sony APR5000 - minimal compression
  ratio = 1/4.0;
  attack = exp( threshDB / (10*srate/1000) / c);
  release = exp( threshDB / (200*srate/1000) / c )
);

slider7 == 3 ? ( // Otari MTR90 - balanced
  ratio = 1/2.0;
  attack = exp( threshDB / (6*srate/1000) / c);
  release = exp( threshDB / (120*srate/1000) / c )
);

slider7 == 4 ? ( // Basic Tape - original settings
  ratio = 1/1.8;
  attack = exp( threshDB / (5*srate/1000) / c);
  release = exp( threshDB / (100*srate/1000) / c )
);

@gfx 650 400

// Calculate scale factor based on window size
base_width = 500;
base_height = 320;
scale_x = gfx_w / base_width;
scale_y = gfx_h / base_height;
scale = min(scale_x, scale_y);
scale = max(0.5, min(scale, 3.0));

// Revox A77 inspired GUI
// Silver-grey background
gfx_r = 0.85; gfx_g = 0.85; gfx_b = 0.85;
gfx_rect(0, 0, gfx_w, gfx_h);

// Title panel
gfx_r = 0.72; gfx_g = 0.72; gfx_b = 0.72;
gfx_rect(20 * scale, 20 * scale, (gfx_w - 40 * scale), 50 * scale);

// Control panel
gfx_rect(20 * scale, 90 * scale, (gfx_w - 40 * scale), 280 * scale);

// Title text
gfx_r = 0; gfx_g = 0; gfx_b = 0;
gfx_setfont(1, "Arial", max(8, 18 * scale), 'b');
gfx_x = 40 * scale; gfx_y = 32 * scale;
gfx_drawstr("TapeEmu");

gfx_setfont(1, "Arial", max(8, 12 * scale));
gfx_x = 40 * scale; gfx_y = 52 * scale;
gfx_drawstr("Professional Tape Machine");

// Red accent
gfx_r = 0.55; gfx_g = 0; gfx_b = 0;
gfx_rect(gfx_w - 80 * scale, 30 * scale, 40 * scale, 4 * scale);

// Draw knobs - Top row
knob_y = 140 * scale;
knob_size = 30 * scale;

// Input/Saturation knob
knob_x = 80 * scale;
gfx_r = 0.125; gfx_g = 0.125; gfx_b = 0.125;
gfx_circle(knob_x, knob_y, knob_size/2, 1);
knob_position = slider1 / 178; // Normalize 0-178 to 0-1
angle = -2.356 + knob_position * 4.712;
ind_x = knob_x + sin(angle) * (knob_size/2 - 3);
ind_y = knob_y - cos(angle) * (knob_size/2 - 3);
gfx_r = 1; gfx_g = 1; gfx_b = 1;
gfx_line(knob_x, knob_y, ind_x, ind_y, 2);
// Label
gfx_r = 0; gfx_g = 0; gfx_b = 0;
gfx_setfont(1, "Arial", max(8, 10 * scale));
gfx_measurestr("INPUT", label_w, label_h);
gfx_x = knob_x - label_w/2; gfx_y = knob_y + 25 * scale;
gfx_drawstr("INPUT");
// Value
gfx_setfont(1, "Arial", max(7, 9 * scale));
gfx_x = knob_x - 15 * scale; gfx_y = knob_y + 40 * scale;
gfx_printf("%.0f%%", slider1);

// Output knob
knob_x = 160 * scale;
gfx_r = 0.125; gfx_g = 0.125; gfx_b = 0.125;
gfx_circle(knob_x, knob_y, knob_size/2, 1);
knob_position = (slider2 + 50) / 50; // Normalize -50 to 0 to 0-1
angle = -2.356 + knob_position * 4.712;
ind_x = knob_x + sin(angle) * (knob_size/2 - 3);
ind_y = knob_y - cos(angle) * (knob_size/2 - 3);
gfx_r = 1; gfx_g = 1; gfx_b = 1;
gfx_line(knob_x, knob_y, ind_x, ind_y, 2);
// Label
gfx_r = 0; gfx_g = 0; gfx_b = 0;
gfx_setfont(1, "Arial", max(8, 10 * scale));
gfx_measurestr("OUTPUT", label_w, label_h);
gfx_x = knob_x - label_w/2; gfx_y = knob_y + 25 * scale;
gfx_drawstr("OUTPUT");
// Value
gfx_setfont(1, "Arial", max(7, 9 * scale));
gfx_x = knob_x - 15 * scale; gfx_y = knob_y + 40 * scale;
gfx_printf("%+.0fdB", slider2);

// Noise knob
knob_x = 240 * scale;
gfx_r = 0.125; gfx_g = 0.125; gfx_b = 0.125;
gfx_circle(knob_x, knob_y, knob_size/2, 1);
knob_position = (slider3 + 105) / 45; // Normalize -105 to -60 to 0-1
angle = -2.356 + knob_position * 4.712;
ind_x = knob_x + sin(angle) * (knob_size/2 - 3);
ind_y = knob_y - cos(angle) * (knob_size/2 - 3);
gfx_r = 1; gfx_g = 1; gfx_b = 1;
gfx_line(knob_x, knob_y, ind_x, ind_y, 2);
// Label
gfx_r = 0; gfx_g = 0; gfx_b = 0;
gfx_setfont(1, "Arial", max(8, 10 * scale));
gfx_measurestr("NOISE", label_w, label_h);
gfx_x = knob_x - label_w/2; gfx_y = knob_y + 25 * scale;
gfx_drawstr("NOISE");
// Value
gfx_setfont(1, "Arial", max(7, 9 * scale));
gfx_x = knob_x - 15 * scale; gfx_y = knob_y + 40 * scale;
gfx_printf("%.0fdB", slider3);

// Head Bump knob
knob_x = 320 * scale;
gfx_r = 0.125; gfx_g = 0.125; gfx_b = 0.125;
gfx_circle(knob_x, knob_y, knob_size/2, 1);
knob_position = (slider4 + 18) / 30; // Normalize -18 to +12 to 0-1
angle = -2.356 + knob_position * 4.712;
ind_x = knob_x + sin(angle) * (knob_size/2 - 3);
ind_y = knob_y - cos(angle) * (knob_size/2 - 3);
gfx_r = 1; gfx_g = 1; gfx_b = 1;
gfx_line(knob_x, knob_y, ind_x, ind_y, 2);
// Label
gfx_r = 0; gfx_g = 0; gfx_b = 0;
gfx_setfont(1, "Arial", max(8, 10 * scale));
gfx_measurestr("HEAD BUMP", label_w, label_h);
gfx_x = knob_x - label_w/2; gfx_y = knob_y + 25 * scale;
gfx_drawstr("HEAD BUMP");
// Value
gfx_setfont(1, "Arial", max(7, 9 * scale));
gfx_x = knob_x - 15 * scale; gfx_y = knob_y + 40 * scale;
gfx_printf("%+.1fdB", slider4);

// Bottom row knobs
knob_y2 = 220 * scale;

// Tape Age knob
knob_x = 80 * scale;
gfx_r = 0.125; gfx_g = 0.125; gfx_b = 0.125;
gfx_circle(knob_x, knob_y2, knob_size/2, 1);
angle = -2.356 + (slider5 / 100) * 4.712;
ind_x = knob_x + sin(angle) * (knob_size/2 - 3);
ind_y = knob_y2 - cos(angle) * (knob_size/2 - 3);
gfx_r = 1; gfx_g = 1; gfx_b = 1;
gfx_line(knob_x, knob_y2, ind_x, ind_y, 2);
// Label
gfx_r = 0; gfx_g = 0; gfx_b = 0;
gfx_setfont(1, "Arial", max(8, 10 * scale));
gfx_measurestr("TAPE AGE", label_w, label_h);
gfx_x = knob_x - label_w/2; gfx_y = knob_y2 + 25 * scale;
gfx_drawstr("TAPE AGE");
// Value
gfx_setfont(1, "Arial", max(7, 9 * scale));
gfx_x = knob_x - 12 * scale; gfx_y = knob_y2 + 40 * scale;
gfx_printf("%.0f%%", slider5);

// Stereo Width knob
knob_x = 160 * scale;
gfx_r = 0.125; gfx_g = 0.125; gfx_b = 0.125;
gfx_circle(knob_x, knob_y2, knob_size/2, 1);
angle = -2.356 + (slider6 / 200) * 4.712;
ind_x = knob_x + sin(angle) * (knob_size/2 - 3);
ind_y = knob_y2 - cos(angle) * (knob_size/2 - 3);
gfx_r = 1; gfx_g = 1; gfx_b = 1;
gfx_line(knob_x, knob_y2, ind_x, ind_y, 2);
// Label
gfx_r = 0; gfx_g = 0; gfx_b = 0;
gfx_setfont(1, "Arial", max(8, 10 * scale));
gfx_measurestr("STEREO WIDTH", label_w, label_h);
gfx_x = knob_x - label_w/2; gfx_y = knob_y2 + 25 * scale;
gfx_drawstr("STEREO WIDTH");
// Value
gfx_setfont(1, "Arial", max(7, 9 * scale));
gfx_x = knob_x - 12 * scale; gfx_y = knob_y2 + 40 * scale;
gfx_printf("%.0f%%", slider6);

// Wow/Flutter knob
knob_x = 240 * scale;
gfx_r = 0.125; gfx_g = 0.125; gfx_b = 0.125;
gfx_circle(knob_x, knob_y2, knob_size/2, 1);
angle = -2.356 + (slider8 / 100) * 4.712;
ind_x = knob_x + sin(angle) * (knob_size/2 - 3);
ind_y = knob_y2 - cos(angle) * (knob_size/2 - 3);
gfx_r = 1; gfx_g = 1; gfx_b = 1;
gfx_line(knob_x, knob_y2, ind_x, ind_y, 2);
// Label
gfx_r = 0; gfx_g = 0; gfx_b = 0;
gfx_setfont(1, "Arial", max(8, 10 * scale));
gfx_measurestr("WOW/FLUTTER", label_w, label_h);
gfx_x = knob_x - label_w/2; gfx_y = knob_y2 + 25 * scale;
gfx_drawstr("WOW/FLUTTER");
// Value
gfx_setfont(1, "Arial", max(7, 9 * scale));
gfx_x = knob_x - 12 * scale; gfx_y = knob_y2 + 40 * scale;
gfx_printf("%.0f%%", slider8);

// Tape Machine selector (fader style)
fader_x = 400 * scale;
fader_y = knob_y - 10 * scale;
fader_height = 100 * scale;
fader_width = 12 * scale;

// Fader track
gfx_r = 0.125; gfx_g = 0.125; gfx_b = 0.125;
gfx_rect(fader_x - fader_width/2, fader_y, fader_width, fader_height);

// Fader handle
handle_y = fader_y + (slider7 / 4) * (fader_height - 6) + 3;
gfx_r = 0.55; gfx_g = 0; gfx_b = 0;
gfx_rect(fader_x - fader_width/2 + 1, handle_y - 3, fader_width - 2, 6);

// Machine name
gfx_r = 0; gfx_g = 0; gfx_b = 0;
gfx_setfont(1, "Arial", max(7, 9 * scale), 'b');
machine_text = "";
slider7 == 0 ? machine_text = "STUDER";
slider7 == 1 ? machine_text = "AMPEX";
slider7 == 2 ? machine_text = "SONY";
slider7 == 3 ? machine_text = "OTARI";
slider7 == 4 ? machine_text = "BASIC";
gfx_measurestr(machine_text, machine_w, machine_h);
gfx_x = fader_x - machine_w/2; gfx_y = fader_y + fader_height + 8 * scale;
gfx_drawstr(machine_text);

// Label
gfx_setfont(1, "Arial", max(6, 8 * scale));
gfx_measurestr("TAPE MACHINE", machine_label_w, machine_label_h);
gfx_x = fader_x - machine_label_w/2; gfx_y = fader_y + fader_height + 25 * scale;
gfx_drawstr("TAPE MACHINE");

// Status display area
gfx_r = 0.6; gfx_g = 0.6; gfx_b = 0.6;
gfx_rect(40 * scale, 310 * scale, (gfx_w - 80 * scale), 50 * scale);

// Status text
gfx_r = 0; gfx_g = 0; gfx_b = 0;
gfx_setfont(1, "Arial", max(6, 8 * scale));
gfx_x = 50 * scale; gfx_y = 325 * scale;

// Show current machine character
slider7 == 0 ? status_text = "Studer A800 - Warm & Musical" :
slider7 == 1 ? status_text = "Ampex ATR102 - Punchy & Colored" :
slider7 == 2 ? status_text = "Sony APR5000 - Clean & Modern" :
slider7 == 3 ? status_text = "Otari MTR90 - Balanced & Professional" :
status_text = "Basic Tape - Generic Emulation";

gfx_drawstr(status_text);

// Show active effects
gfx_x = 50 * scale; gfx_y = 340 * scale;
effects_text = "";
slider5 > 0 ? effects_text = effects_text + "AGE " + slider5 + "% ";
slider8 > 0 ? effects_text = effects_text + "W&F " + slider8 + "% ";
slider6 != 100 ? effects_text = effects_text + "WIDTH " + slider6 + "%";
gfx_drawstr(effects_text);

// Mouse interaction system
mouse_cap == 1 && last_mouse_cap == 0 ? (
    // Mouse click started - check what was clicked
    knob_hit = 0;
    hit_distance = 25 * scale; // Larger hit area
    
    // Check top row knobs
    abs(mouse_x - 80 * scale) <= hit_distance && abs(mouse_y - 140 * scale) <= hit_distance ? (
        slider_to_edit = 1; knob_hit = 1;
    ) : abs(mouse_x - 160 * scale) <= hit_distance && abs(mouse_y - 140 * scale) <= hit_distance ? (
        slider_to_edit = 2; knob_hit = 1;
    ) : abs(mouse_x - 240 * scale) <= hit_distance && abs(mouse_y - 140 * scale) <= hit_distance ? (
        slider_to_edit = 3; knob_hit = 1;
    ) : abs(mouse_x - 320 * scale) <= hit_distance && abs(mouse_y - 140 * scale) <= hit_distance ? (
        slider_to_edit = 4; knob_hit = 1;
    ) :
    
    // Check bottom row knobs
    abs(mouse_x - 80 * scale) <= hit_distance && abs(mouse_y - 220 * scale) <= hit_distance ? (
        slider_to_edit = 5; knob_hit = 1;
    ) : abs(mouse_x - 160 * scale) <= hit_distance && abs(mouse_y - 220 * scale) <= hit_distance ? (
        slider_to_edit = 6; knob_hit = 1;
    ) : abs(mouse_x - 240 * scale) <= hit_distance && abs(mouse_y - 220 * scale) <= hit_distance ? (
        slider_to_edit = 8; knob_hit = 1;
    ) :
    
    // Check tape machine fader - wider hit area
    abs(mouse_x - 400 * scale) <= 20 * scale && mouse_y >= 130 * scale && mouse_y <= 230 * scale ? (
        // Calculate new position
        fader_pos = (mouse_y - 130 * scale) / (100 * scale);
        fader_pos = max(0, min(1, fader_pos));
        slider7 = floor(fader_pos * 4);
        update_audio_vars(); // Update audio processing variables
        knob_hit = 1;
    );
    
    // Initialize drag
    knob_hit ? (
        last_mouse_y = mouse_y;
        drag_start_y = mouse_y;
    ) : (
        slider_to_edit = 0;
    );
);

// Handle dragging
mouse_cap == 1 && slider_to_edit > 0 && slider_to_edit != 7 ? (
    mouse_delta = last_mouse_y - mouse_y; // Inverted for natural feel
    sensitivity = 1.0; // Base sensitivity
    
    // Apply changes with proper scaling
    slider_to_edit == 1 ? ( // Input/Saturation (0-178)
        new_val = slider1 + mouse_delta * sensitivity;
        slider1 = max(0, min(178, new_val));
        update_audio_vars(); // Update audio processing variables
    ) : slider_to_edit == 2 ? ( // Output (-50 to 0)
        new_val = slider2 + mouse_delta * 0.25;
        slider2 = max(-50, min(0, new_val));
        update_audio_vars(); // Update audio processing variables
    ) : slider_to_edit == 3 ? ( // Noise (-105 to -60)
        new_val = slider3 + mouse_delta * 0.25;
        slider3 = max(-105, min(-60, new_val));
        update_audio_vars(); // Update audio processing variables
    ) : slider_to_edit == 4 ? ( // Head Bump (-18 to +12)
        new_val = slider4 + mouse_delta * 0.15;
        slider4 = max(-18, min(12, new_val));
        update_audio_vars(); // Update audio processing variables
    ) : slider_to_edit == 5 ? ( // Tape Age (0-100)
        new_val = slider5 + mouse_delta * 0.5;
        slider5 = max(0, min(100, new_val));
        update_audio_vars(); // Update audio processing variables
    ) : slider_to_edit == 6 ? ( // Stereo Width (0-200)
        new_val = slider6 + mouse_delta;
        slider6 = max(0, min(200, new_val));
        update_audio_vars(); // Update audio processing variables
    ) : slider_to_edit == 8 ? ( // Wow/Flutter (0-100)
        new_val = slider8 + mouse_delta * 0.5;
        slider8 = max(0, min(100, new_val));
        update_audio_vars(); // Update audio processing variables
    );
    
    last_mouse_y = mouse_y;
);

// Reset when mouse released
mouse_cap == 0 ? (
    slider_to_edit = 0;
);

last_mouse_cap = mouse_cap;
