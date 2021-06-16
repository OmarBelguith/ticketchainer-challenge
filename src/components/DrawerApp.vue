<template>
  <canvas @click="drawPolygon" id="drawer" ref="canvas" @resize="resizeCanvas"></canvas>
</template>

<script>
export default {
  name: "DrawerApp",
  data() {
    return {
      pointCoordinates: [],
      firstPointCoordinates: [],
      index: 2, // Initialized to 2 to go from the second point to the previous point
    };
  },
  mounted() {
    // Setting canvas to width and height of window
    this.resizeCanvas()
  },
  methods: {
    drawPolygon (event) {
      const context = this.$refs.canvas.getContext("2d"); // Get Context of canvas
      const { clientX, clientY } = event; // Extract the mouse position from the event
      this.drawDot(context, clientX, clientY); // Draw The dot in that position
      this.storePointCoordinates(clientX, clientY); // Store Points for conecting them after
      if (this.pointCoordinates.length == 2) { // Check if they are the first Points
        console.log('here');
        this.storeFirstPointCoordinates(clientX, clientY);
      }else if (this.pointCoordinates.length > 2) { // draw a line between the two coordinates
        console.log('here');
        this.drawLine(context);
        this.index += 2; // move index forward for next Point
        if (this.checkDotInRadius(clientX, clientY)) { // If the new Point is in the Radius Of the First Point Re-initialize Everything 
          this.pointCoordinates = [];
          this.firstPointCoordinates = []; // Make Room for a new Polygon
          this.index = 2
        }
      }
    },
    drawDot (context, mouseX, mouseY) {
      const radius = 5;
      context.beginPath();
      context.arc(mouseX, mouseY, radius, 0, 2 * Math.PI, false);
      context.fillStyle = "green";
      context.fill();
      context.lineWidth = 2;
      context.strokeStyle = "#003300";
      context.stroke();
    },
    drawLine (context) {
      console.log(context);
      context.beginPath(); // Start a new path
      context.moveTo(this.pointCoordinates[this.index], this.pointCoordinates[this.index + 1]); // Move the pen to where Mouse was clicked
      context.lineTo(this.pointCoordinates[this.index - 2], this.pointCoordinates[this.index - 1]); // Draw a line to previous Mouse Click Point Coordinates
      context.stroke();
    },
    storeFirstPointCoordinates (mouseX, mouseY) {
      this.firstPointCoordinates.push(mouseX, mouseY);
    },
    storePointCoordinates (mouseX, mouseY) {
      this.pointCoordinates.push(mouseX, mouseY);
    },
    checkDotInRadius (mouseX, mouseY) {
      return (this.firstPointCoordinates[0] - mouseX < 15 && this.firstPointCoordinates[0] - mouseX > -15) 
      && (this.firstPointCoordinates[1] - mouseY < 15 && this.firstPointCoordinates[1] - mouseY > -15 )
    },
    resizeCanvas () {
      this.$refs.canvas.width = window.innerWidth;
      this.$refs.canvas.height = window.innerHeight;
    }
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
