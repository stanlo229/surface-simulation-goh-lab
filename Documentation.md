# Documentation
The style of documentation only contains an explanation of important snippets of code <br />
**Note: read this after reading the README and summary report.**
## v1 - Cartesian Coordinate
### x_binary_search
Looks at a point on the bacteria, and finds the closest point on the surface (which is normally directly underneath)
```bash
def x_binary_search(node, x_nodes):
    if len(x_nodes) == 1:
        return x_nodes[0]
    else:
        x = node[0]
        mid_1 = int(len(x_nodes)/2) - 1
        mid_2 = int(len(x_nodes)/2)
        dist_1 = ((x_nodes[mid_1] - x)**2)
        dist_2 = ((x_nodes[mid_2] - x)**2)
        if dist_1 < dist_2:
            return x_binary_search(node, x_nodes[:mid_2]) #since end is exclusive, while start is inclusive
        else:
            return x_binary_search(node, x_nodes[mid_2:])
```
Uses recursion to quickly find the closest point.

### Rotate 
During bacteria-surface interaction, the bacteria rotates around its axis. (refer to picture shown below)
![rotate](https://github.com/stanlo229/Surface-Simulation/blob/main/rotate.png)
```bash
def Rotate(Particle, Surface, temp_dict, circle_list):
    y = 0 #key for lowest point
    min_val = 100
    for i in temp_dict:
            if i[2] < min_val:
                min_val = i[2]
                y = i
                
    bact_coord = temp_dict
    surf_coord = list(Surface.coordinates)
    temp_bact = copy.copy(temp_dict)
    #change how large of the bottom portion of the cylinder to measure
    search_range = 10  # NOTE! must be 1/4 of size of circle because it searches up and down
    energy_per_circle = []
    first_circle = 1
    .
    .
    .
```
### Particle(object)
This is the class that creates the bacteria. 
```bash
 def __init__(self, radius, length, height, position, seed):
```
The two ambiguous inputs are height and position. <br />
height - the bottom of the bacteria starts at (height) in the z-axis <br />
position - the position of the initial bacteria on the surface, inputs can range from 0 to maximum surface size (self.size_x) . If "R" is the input, the bacteria will be placed randomly. 

### Changing domain size

```bash
        #creating domains on cylinder (to change this, refer to GitHub readme)
        for i in range(36):
            index = np.random.randint(1, 1221)
            if (index - 71) % 72 == 0:
                index = index - 2
            elif (index - 70) % 72 == 0:
                index = index - 1
            elif (index % 72) == 0:
                index = index + 1
            #form octagons (circles)
            key_1 = self.circle_coords[index]
            key_2 = self.circle_coords[index+1]
            key_3 = self.circle_coords[index-1+72]
            key_4 = self.circle_coords[index+72]
            key_5 = self.circle_coords[index+1+72]
            key_6 = self.circle_coords[index+2+72]
            key_7 = self.circle_coords[index-1+144]
            key_8 = self.circle_coords[index+144]
            key_9 = self.circle_coords[index+1+144]
            key_10 = self.circle_coords[index+2+144]
            key_11 = self.circle_coords[index+216]
            key_12 = self.circle_coords[index+1+216]
            self.coords[key_1] = "B"
            self.coords[key_2] = "B"
            self.coords[key_3] = "B"
            self.coords[key_4] = "B"
            self.coords[key_5] = "B"
            self.coords[key_6] = "B"
            self.coords[key_7] = "B"
            self.coords[key_8] = "B"
            self.coords[key_9] = "B"
            self.coords[key_10] = "B"
            self.coords[key_11] = "B"
            self.coords[key_12] = "B"
```
The domains are created by randomly choosing a point on the bacteria, and then expanding out from that point. The numbers added to "index" are the points above, below, or beside the chosen point. Multiples of 72 are used here because the bacteria (cylinder) is made by 20 rings of 72 points each. *It's definitely a bit confusing at first, but you'll get the hang of it once you see the visuals and can envision the code produced in your head*

### Visualization
Creates the interactive visualization of the bacteria. Does not require any modification.
```bash
#3D Color coordinated (points-only)
        #Note: the patchiness is off angled to begin with, but doesn't matter if you rotate through all of it
        Ax = []
        Ay = []
        Az = []
        Bx = []
        By = []
        Bz = []
        Cx = []
        Cy = [] 
        Cz = []
        Dx = []
        Dy = []
        Dz = []

        for i in self.coords:
            if self.coords[i] == 'A':
                Ax.append(i[0])
                Ay.append(i[1])
                Az.append(i[2])
            elif self.coords[i] == 'B':
                Bx.append(i[0])
                By.append(i[1])
                Bz.append(i[2])
            elif self.coords[i] == 'C':
                Cx.append(i[0])
                Cy.append(i[1])
                Cz.append(i[2])
            else:
                Dx.append(i[0])
                Dy.append(i[1])
                Dz.append(i[2])
        
        ax2.scatter(Ax, Ay, Az, c = 'red', edgecolors='b', label = 'A')
        ax2.scatter(Bx, By, Bz, c = 'blue', edgecolors='b', label = 'B')
        ax2.scatter(Cx, Cy, Cz, c = 'green', edgecolors='b', label = 'C')
        ax2.scatter(Dx, Dy, Dz, c = 'black', edgecolors='b', label = 'D')
        ax2.legend(loc = "upper right")
        
        ax2.set_xlabel("X")
        ax2.set_ylabel("Y")
        ax2.set_zlabel("Z")
        #ax2.title.set_text("Number of Sims: " + str(self.num_sim))
        
        # Customize the z axis.
        ax2.set_zlim(0, 5)
        ax2.zaxis.set_major_locator(LinearLocator(10))
        ax2.zaxis.set_major_formatter(FormatStrFormatter('%.02f'))
        
        # rotate the axes and update
        for angle in range(0, 360):
           ax2.view_init(30, 40)
```

### Translation part of interact(self, Surface, search_range)
Moves the bacteria across the surface in a pre-defined number of increments (can be adjusted). 
```bash
 for i in np.linspace(start_x, end_x, increment_x):
            a = i - self.position
            x_bound.append(a)
        
        for i in np.linspace(start, end, increment_y):
            a = i - self.position
            y_bound.append(a)
        
        x_bound = [round(num,4) for num in x_bound]
        y_bound = [round(num,4) for num in y_bound]
        #print(x_bound, y_bound)
        
        translation_energy = []
        translation_index = []
        translation_angle = []
        
        #loop stuff
        #add loop through all x coords, and y coords addition to existing coordinates while marking all the
        counter = 0
        while counter < len(x_bound):
            counter2 = 0
            x = x_bound[counter]
            tempx = copy.copy(self.coords)
            newx = {}
            for coord in self.coords:
                old_key = copy.copy(coord)
                mod_key = copy.copy(coord)
                mod_key = list(mod_key)
                mod_key[0] += x
                mod_key = tuple(mod_key)
                newx[mod_key] = tempx[old_key]
            while counter2 < len(y_bound):
                y = y_bound[counter2]
                tempy = copy.copy(newx)
                newy = {}
                for coord in newx:
                    old_key = copy.copy(coord)
                    mod_key = copy.copy(coord)
                    mod_key = list(mod_key)
                    mod_key[1] += y
                    mod_key = tuple(mod_key)
                    newy[mod_key] = tempy[old_key]
```

### Rotation in the z-axis
Rotates the bacteria around its z-axis. (Refer to picture shown below) <br />
![rotatez](https://github.com/stanlo229/Surface-Simulation/blob/main/rotatez.png)

```bash
rotation_energy = []
                rotation_index = []
                rotation_angle = []
                rotation_xy = []
                last_one = 1
                rotates = 17 #22.5 degrees per rot
                for y in np.linspace(0 , 2*math.pi, rotates):
                    if last_one != rotates:
                        last_one += 1
                        tempz = copy.copy(newy)
                        first_line = line_list[0]
                        x11 = first_line[0]
                        x12 = first_line[len(first_line)-1]
                        cx = (x11[0] + x12[0]) / 2
                        cy = (x11[1] + x12[1]) / 2
                        line1 = []
                        line_ct = 0
                        tempxyz = {}
                        for line in line_list:
                            #cx = (line[0][0] + line[len(line)-1][0]) / 2
                            #cy = (line[0][1] + line[len(line)-1][1]) / 2
                            for i in line:
                                x_new = ((i[0]-cx) * math.cos(y) - (i[1]-cy) * math.sin(y)) + cx
                                y_new = ((i[0]-cx) * math.sin(y) + (i[1]-cy) * math.cos(y)) + cy
                                new_key = (x_new, y_new, i[2])
                                tempxyz[new_key] = tempz[i]
                                if line_ct == 0:
                                    line1.append(new_key)
                            line_ct += 1
                        
                        tempz_labels = tempxyz.keys()
                        a = []
                        circle_list = []
                        #must sort by a diagonal line, hmmm
                        pt_1 = line1[0]
                        pt_last = line1[len(line1)-1]
                        if (round(pt_last[0],5) - round(pt_1[0],5)) == 0:
                            #compile circle keys (x,y,z) sorted by Y
                            for xyz in tempz_labels:
                                if round(xyz[1], 5) not in a:
                                    a.append(round(xyz[1], 5))
                                    circle_list.append([xyz])
                                else:
                                    index = a.index(round(xyz[1], 5))
                                    circle_list[index].append(xyz)
                        elif (round(pt_last[1],5) - (round(pt_1[1],5))) == 0:
                            #compile circle keys (x,y,z) sorted by X
                            for xyz in tempz_labels:
                                if round(xyz[0], 5) not in a:
                                    a.append(round(xyz[0], 5))
                                    circle_list.append([xyz])
                                else:
                                    index = a.index(round(xyz[0], 5))
                                    circle_list[index].append(xyz)
                        else:
                            perp_slope = (pt_last[1] - pt_1[1]) / (pt_last[0] - pt_1[0])
                            slope = - 1 / perp_slope
                            counted = 0
                            counted2 = 0
                            for pt in line1:
                                circle_list.append([pt])
                                counta = 0
                                for xyz in tempz_labels:
                                    if xyz != pt:
                                        temp_slope = round((xyz[1] - pt[1])/(xyz[0] - pt[0]), 6)
                                        #counted2 += 1
                                        #print(pt, list(tempz_labels)[1360], list(tempz_labels)[1419])
                                        #print(temp_slope, round(slope,6))
                                        #print(temp_slope, round(slope, 6)) #missing 40... 1420-1380
                                        if temp_slope == round(slope, 6) or math.isnan(temp_slope):
                                            circle_list[counted].append(xyz)
                                            counted2 += 1
                                    counta += 1
                                #print("woot", counta)
                                counted += 1
                        circle_coords = []
                        #form into one list
                        for circle in circle_list:
                            for pos in circle:
                                circle_coords.append(pos)
```
Once this is done, the energy gets calculated and the minimum energy is recorded.

### Rest of the Code for Particle()
For the most part, it is a repeat of what was shown before but instead of scanning through each configuration, it recreates the configuration with the lowest energy (which can be used for visualization, etc.)

### Surface(self, size_x, size_y)
```bash
def __init__(self, size_x, size_y):
        self.labels = []
        self.coordinates = {}
        self.size_x = size_x
        self.size_y = size_y
        points_x = 80
        points_y = 160
        np.random.seed(1)
        
        #makes surface
        self.x = np.linspace(0, self.size_x, points_x)
        self.y = np.linspace(0, self.size_y, points_y)
        
        xy, yx = np.meshgrid(self.x, self.y)
        xz = xy*0
        
        for i in range(len(xy)):
            for k in range(len(xy[i])):
                self.labels.append((xy[i][k], yx[i][k], xz[i][k]))
        
        #create a gradient from A --> B
        gradient = np.linspace(0, 1, points_y)
        line_ct = 0
        counter = 1
        firstlines = 0
        while counter <= len(self.labels):
            key = self.labels[counter-1]
            conc = [gradient[line_ct], 1 - gradient[line_ct]]
            if counter % points_x == 0:
                if firstlines < 50:
                    firstlines += 1
                elif line_ct < 99:
                    line_ct += 1
                #print(conc)
            if line_ct < 100:
                self.coordinates[key] = np.random.choice(['A', 'B'], size = 1, p = conc)
            else:
                self.coordinates[key] = "A"
            counter += 1
```
Modifying the *sizes* and the *points* variables will change the physical size and the number of points it takes to fill up that size. So, a small size with lots of points will create a dense surface.

## v2 - Matrix Simulation
The code in v2 is much cleaner, so there will be less documentation because most things are written with comments.

### Changing domain size (found in main_surface, and a similar version in main_bacteria)
Like v1, the domains are created by choosing a point randomly and then adding to the index to change the points around the chosen point. <br />
The difference is that the matrix creates a surface with all positive, and then adds negative domains. So, the chosen point must be checked if it is negative or positive. If it's negative, we cannot have duplicate domains so it'll loop through again and choose another point. 
```bash
for k in range(1):
                #first, make surface positive
                surf_1d = np.ones((tot))
                num_domains = int(((10500*10500)*0.2)/5)
                print("neg: ", num_domains)
                dom = 0
                while dom < num_domains:
                    index = np.random.randint(y_ax, tot - y_ax - 2) #randomly pick an index
                    if index % y_ax == 0 or index+1 % y_ax == 0 or index-1 % y_ax == 0:
                        continue

                    elif surf_1d[index - y_ax] == -1 or \
                         surf_1d[index + y_ax] == -1:
                        continue

                    dom_bool = False
                    for i in range(-1, 2):
                        if surf_1d[index + i] == -1:
                            dom_bool = True

                    if dom_bool:
                        continue
                    else:
                        for i in range(-1, 2):
                            surf_1d[index+i] = -1
                            surf_1d[index+y_ax] = -1
                            surf_1d[index-y_ax] = -1
                        dom += 1
                if k == 0:
                    self.surf_1d = surf_1d
                else:
                    self.surf_1d = np.append(self.surf_1d, surf_1d, axis = 0)                
            self.surf_2d = np.reshape(self.surf_1d, (-1, 10500))
```

#### If there are any questions: please contact stanley.lo@mail.utoronto.ca with the email title "Surface Simulation Troubleshooting"
