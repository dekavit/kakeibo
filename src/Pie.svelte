<script>
  export let size = 200;
  export let percent = 0;
  export let percent1 = 0;
  export let percent2 = 0;
  export let percent3 = 0;
  export let bgColor = "cornflowerblue";
  export let fgColor = "orange";
  export let fg1Color = "red";
  export let fg2Color = "blue";
  export let fg3Color = "green";
  export let bg2Color = "white";

  $: viewBox = `0 0 ${size} ${size}`;

  $: radius = size / 2;
  $: halfCircumference = Math.PI * radius;
  $: pieSize = halfCircumference * (percent / 100);
  $: pieSize1 = halfCircumference * (percent1 / 100);
  $: pieSize2 = halfCircumference * (percent2 / 100);
  $: pieSize3 = halfCircumference * (percent3 / 100);
  $: dashArray = `0 ${halfCircumference - pieSize} ${pieSize}`;
  $: dashArray1 = `0 ${halfCircumference - pieSize1} ${pieSize1}`;
  $: dashArray2 = `0 ${halfCircumference - pieSize2} ${pieSize2}`;
  $: dashArray3 = `0 ${halfCircumference - pieSize3} ${pieSize3}`;
  $: dashArray4 = `0 ${halfCircumference}`;
</script>

<svg width={size} height={size} {viewBox}>
  <circle r={radius} cx={radius} cy={radius} fill={bgColor} />
  <circle
    r={radius / 2}
    cx={radius}
    cy={radius}
    fill={bgColor}
    stroke={fgColor}
    stroke-width={radius}
    stroke-dasharray={dashArray}
  />
  <circle
    r={radius / 2}
    cx={radius}
    cy={radius}
    fill={bgColor}
    stroke={fg1Color}
    stroke-width={radius}
    stroke-dasharray={dashArray1}
  />
  <circle
    r={radius / 2}
    cx={radius}
    cy={radius}
    fill={bgColor}
    stroke={fg2Color}
    stroke-width={radius}
    stroke-dasharray={dashArray2}
  />
  <circle
    r={radius / 2}
    cx={radius}
    cy={radius}
    fill={bgColor}
    stroke={fg3Color}
    stroke-width={radius}
    stroke-dasharray={dashArray3}
  />
  <circle
    r={radius / 2}
    cx={radius}
    cy={radius}
    fill={bg2Color}
    stroke={bg2Color}
    stroke-width={radius}
    stroke-dasharray={dashArray4}
  />
</svg>
