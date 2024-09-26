# Advanced-Practise-Backbone

A good chunk of the code was not made by me.

My code includes:

x = slice.vertices[:,0]
y = slice.vertices[:,1]
center_y = slice.centroid[1]

#INDEXES
above_centroid = np.where(y > center_y)
below_centroid = np.where(y < center_y)[0]
top = np.argmax(y)
bot = np.argmin(y)
thoracic_apex = np.argmax(x)
lumbar_apex = np.argmin(x[below_centroid])
lumbar_apex = np.where(x == x[below_centroid][lumbar_apex])[0][0]

ax = plt.gca()

midpoint = np.abs(x[thoracic_apex]) - np.abs(x[lumbar_apex])

# midpoint = x[top]
print(midpoint)

top_line = ax.axhline(y = y[top], color='black')
midpoint_line = ax.axvline(x = midpoint, color='black')
centroid_line = ax.axhline(y = center_y, color='black')
bot_line = ax.axhline(y = y[bot], color='black')
thoracic_apex_line = ax.axhline(y = y[thoracic_apex], color = 'orange')
lumbar_apex_line = ax.axhline(y = y[lumbar_apex], color='yellow')

slice.show()

ax = plt.gca()

ax.scatter(x = x[thoracic_apex], y = y[thoracic_apex])
ax.scatter(x = x[lumbar_apex], y = y[lumbar_apex])
slice.show()

plt.scatter(x = x[below_centroid], y = y[below_centroid])
plt.scatter(x = x[above_centroid], y = y[above_centroid])

'''
Thoracic Kyphosis Angle calculation

Notes:
Top = T1
thoracic_apex = Apex
Centroid = T12
'''

top_spot = (x[top], y[top])
thoracic_apex_spot = (x[thoracic_apex], y[thoracic_apex])
centroid_spot = (slice.centroid[0], slice.centroid[1])
print(f'Top Spot:           {top_spot}')
print(f'Thoracic Apex Spot: {thoracic_apex_spot}')
print(f'Centroid Spot:      {centroid_spot}')

x1 = [thoracic_apex_spot[0], top_spot[0]]
y1 = [thoracic_apex_spot[1], top_spot[1]]

x2 = [thoracic_apex_spot[0], centroid_spot[0]]
y2 = [thoracic_apex_spot[1], centroid_spot[1]]

x1 = [top_spot[0], thoracic_apex_spot[0]]
y1 = [top_spot[1], thoracic_apex_spot[1]]

x2 = [centroid_spot[0], thoracic_apex_spot[0]]
y2 = [centroid_spot[1], thoracic_apex_spot[1]]

ax = plt.gca()
plt.xlim(-.6,.6)
plt.ylim(-.6,.6)

spot1 = ax.scatter(x1[0], y1[0])
spot2 = ax.scatter(x1[1], y1[1])
spot3 = ax.scatter(x2[0], y2[0])
spot4 = ax.scatter(x2[1], y2[1])

line1 = ax.plot(x1, y1)
line2 = ax.plot(x2, y2)

# θ = atan2(y2 - y1, x2 - x1)
angle1 = math.atan2(y1[1] - y1[0], x1[1] - x1[0]) * (180 / math.pi) + 90
print(f'{angle1}')
angle2 = math.atan2(y2[1] - y2[0], x2[1] - x2[0]) * (180 / math.pi) + 90
print(angle2)

# θ = 180° - |α - β|
theta = 180 - abs(angle1 - angle2)
print(f'Theta: {theta}')
