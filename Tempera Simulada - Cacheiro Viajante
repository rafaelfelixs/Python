from __future__ import print_function
import math
import random
from simanneal import Annealer


def distance(a, b):
    """Calculates distance between two latitude-longitude coordinates."""
    R = 3963  # radius of Earth (miles)
    lat1, lon1 = math.radians(a[0]), math.radians(a[1])
    lat2, lon2 = math.radians(b[0]), math.radians(b[1])
    return math.acos(math.sin(lat1) * math.sin(lat2) +
                     math.cos(lat1) * math.cos(lat2) * math.cos(lon1 - lon2)) * R


class TravellingSalesmanProblem(Annealer):

    """Test annealer with a travelling salesman problem.
    """

    # pass extra data (the distance matrix) into the constructor
    def __init__(self, state, distance_matrix):
        self.distance_matrix = distance_matrix
        super(TravellingSalesmanProblem, self).__init__(state)  # important!

    def move(self):
        """Swaps two cities in the route."""
        a = random.randint(0, len(self.state) - 1)
        b = random.randint(0, len(self.state) - 1)
        self.state[a], self.state[b] = self.state[b], self.state[a]

    def energy(self):
        """Calculates the length of the route."""
        e = 0
        for i in range(len(self.state)):
            e += self.distance_matrix[self.state[i-1]][self.state[i]]
        return e



if __name__ == '__main__':

    # latitude and longitude for the twenty Ceará cities
    cities = {
        'ABAIARA': (-39.04, -7.35),
        'ACARAPE': (-38.70, -4.22),
        'ACARAÚ': (-40.11, -2.88),
        'ACOPIARA': (-39.45, -6.09),
        'AIUABA': (-40.12, -6.56),
        'CARIRÉ': (-40.47, -3.95),
        'CARIRIAÇU': (-39.28, -7.04),
        'CARIÚS': (-39.49, -6.53),
        'CARNAUBAL': (-40.93, -4.16),
        'CASCAVEL': (-38.23, -4.13),
        'FARIAS BRITO': (-39.57, -6.92),
        'FORQUILHA': (-40.25, -3.79),
        'FORTALEZA': (-38.58, -3.72),
        'FORTIM': (-37.79, -4.44),
        'FRECHEIRINHA': (-40.82, -3.76),
        'ITAITINGA': (-38.52, -3.97),
        'ITAPAGÉ': (-39.58, -3.68),
        'ITAPIPOCA': (-39.58, -3.49),
        'ITAPIÚNA': (-38.92, -4.55),
        'ITAREMA': (-39.91, -2.92)
    }

    # initial state, a randomly-ordered itinerary
    init_state = list(cities.keys())
    random.shuffle(init_state)

    # create a distance matrix
    distance_matrix = {}
    for ka, va in cities.items():
        distance_matrix[ka] = {}
        for kb, vb in cities.items():
            if kb == ka:
                distance_matrix[ka][kb] = 0.0
            else:
                distance_matrix[ka][kb] = distance(va, vb)

    tsp = TravellingSalesmanProblem(init_state, distance_matrix)
    tsp.steps = 100000
    # since our state is just a list, slice is the fastest way to copy
    tsp.copy_strategy = "slice"
    state, e = tsp.anneal()

    while state[0] != 'FORTALEZA':
        state = state[1:] + state[:1]  # rotate FORTALEZA to start

    print()
    print("%i mile route:" % e)
    for city in state:
        print("\t", city
