# davids-restaurant-directory
Dining Rating System
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>David's Restaurant Directory</title>
    <script src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script src="https://unpkg.com/lucide@latest/dist/umd/lucide.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* PWA styles for better mobile experience */
        body {
            -webkit-touch-callout: none;
            -webkit-user-select: none;
            -khtml-user-select: none;
            -moz-user-select: none;
            -ms-user-select: none;
            user-select: none;
            -webkit-tap-highlight-color: transparent;
        }
        
        input[type="text"], input[type="range"] {
            -webkit-user-select: text;
            -moz-user-select: text;
            -ms-user-select: text;
            user-select: text;
        }

        /* Custom slider styles */
        input[type="range"] {
            -webkit-appearance: none;
            appearance: none;
            background: transparent;
            cursor: pointer;
        }

        input[type="range"]::-webkit-slider-track {
            background: #e5e7eb;
            height: 8px;
            border-radius: 4px;
        }

        input[type="range"]::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            background: #3b82f6;
            height: 20px;
            width: 20px;
            border-radius: 50%;
            cursor: pointer;
        }

        input[type="range"]::-moz-range-track {
            background: #e5e7eb;
            height: 8px;
            border-radius: 4px;
            border: none;
        }

        input[type="range"]::-moz-range-thumb {
            background: #3b82f6;
            height: 20px;
            width: 20px;
            border-radius: 50%;
            cursor: pointer;
            border: none;
        }
    </style>
</head>
<body>
    <div id="root"></div>

    <script type="text/babel">
        const { useState } = React;
        const { Star, Plus, Utensils, DollarSign, Heart, AlertTriangle, Package } = lucide;

        const RestaurantRatingApp = () => {
            const [restaurants, setRestaurants] = useState([]);
            const [showAddForm, setShowAddForm] = useState(false);
            const [newRestaurant, setNewRestaurant] = useState({
                name: '',
                location: '',
                isTakeout: false,
                foodQuality: 5,
                annoyanceLevel: 5,
                travelsWell: 5,
                tasteToPrice: 5,
                wantToReturn: 5
            });

            const handleAddRestaurant = () => {
                if (newRestaurant.name.trim()) {
                    const totalScore = newRestaurant.foodQuality + 
                        (newRestaurant.isTakeout ? newRestaurant.travelsWell : newRestaurant.annoyanceLevel) + 
                        newRestaurant.tasteToPrice + 
                        newRestaurant.wantToReturn;
                    const restaurantWithScore = {
                        ...newRestaurant,
                        id: Date.now(),
                        totalScore,
                        dateAdded: new Date().toLocaleDateString()
                    };
                    setRestaurants([restaurantWithScore, ...restaurants]);
                    setNewRestaurant({
                        name: '',
                        location: '',
                        isTakeout: false,
                        foodQuality: 5,
                        annoyanceLevel: 5,
                        travelsWell: 5,
                        tasteToPrice: 5,
                        wantToReturn: 5
                    });
                    setShowAddForm(false);
                }
            };

            const getScoreColor = (score) => {
                if (score >= 32) return 'text-green-600 bg-green-100';
                if (score >= 24) return 'text-yellow-600 bg-yellow-100';
                return 'text-red-600 bg-red-100';
            };

            const ScoreSlider = ({ label, value, onChange, icon: Icon }) => (
                React.createElement('div', { className: 'mb-4' },
                    React.createElement('div', { className: 'flex items-center mb-2' },
                        React.createElement(Icon, { className: 'w-4 h-4 mr-2 text-gray-600' }),
                        React.createElement('label', { className: 'text-sm font-medium text-gray-700' }, label),
                        React.createElement('span', { className: 'ml-auto text-sm font-bold text-blue-600' }, `${value}/10`)
                    ),
                    React.createElement('input', {
                        type: 'range',
                        min: '1',
                        max: '10',
                        value: value,
                        onChange: (e) => onChange(parseInt(e.target.value)),
                        className: 'w-full h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer'
                    })
                )
            );

            return React.createElement('div', { className: 'max-w-4xl mx-auto p-6 bg-gray-50 min-h-screen' },
                React.createElement('div', { className: 'bg-white rounded-lg shadow-lg p-6 mb-6' },
                    React.createElement('div', { className: 'flex items-center justify-between mb-6' },
                        React.createElement('div', { className: 'flex items-center' },
                            React.createElement(Utensils, { className: 'w-8 h-8 mr-3 text-orange-500' }),
                            React.createElement('h1', { className: 'text-3xl font-bold text-gray-800' }, "David's Restaurant Directory")
                        ),
                        React.createElement('button', {
                            onClick: () => setShowAddForm(!showAddForm),
                            className: 'flex items-center px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition-colors'
                        },
                            React.createElement(Plus, { className: 'w-4 h-4 mr-2' }),
                            'Add Restaurant'
                        )
                    ),

                    restaurants.length > 0 && React.createElement('div', { className: 'mb-6 p-4 bg-blue-50 rounded-lg' },
                        React.createElement('h3', { className: 'text-lg font-semibold text-gray-800 mb-2' }, 'Database Stats'),
                        React.createElement('div', { className: 'grid grid-cols-2 md:grid-cols-4 gap-4 text-center' },
                            React.createElement('div', null,
                                React.createElement('div', { className: 'text-2xl font-bold text-blue-600' }, restaurants.length),
                                React.createElement('div', { className: 'text-sm text-gray-600' }, 'Total Restaurants')
                            ),
                            React.createElement('div', null,
                                React.createElement('div', { className: 'text-2xl font-bold text-green-600' },
                                    Math.round(restaurants.reduce((sum, r) => sum + r.totalScore, 0) / restaurants.length)
                                ),
                                React.createElement('div', { className: 'text-sm text-gray-600' }, 'Average Score')
                            ),
                            React.createElement('div', null,
                                React.createElement('div', { className: 'text-2xl font-bold text-purple-600' },
                                    Math.max(...restaurants.map(r => r.totalScore))
                                ),
                                React.createElement('div', { className: 'text-sm text-gray-600' }, 'Highest Score')
                            ),
                            React.createElement('div', null,
                                React.createElement('div', { className: 'text-2xl font-bold text-orange-600' },
                                    restaurants.filter(r => r.wantToReturn >= 8).length
                                ),
                                React.createElement('div', { className: 'text-sm text-gray-600' }, 'Want to Return')
                            )
                        )
                    ),

                    showAddForm && React.createElement('div', { className: 'bg-gray-50 p-6 rounded-lg mb-6' },
                        React.createElement('h2', { className: 'text-xl font-semibold mb-4 text-gray-800' }, 'Add New Restaurant'),
                        
                        React.createElement('div', { className: 'grid md:grid-cols-2 gap-4 mb-4' },
                            React.createElement('input', {
                                type: 'text',
                                placeholder: 'Restaurant Name',
                                value: newRestaurant.name,
                                onChange: (e) => setNewRestaurant({...newRestaurant, name: e.target.value}),
                                className: 'px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent'
                            }),
                            React.createElement('input', {
                                type: 'text',
                                placeholder: 'Location (optional)',
                                value: newRestaurant.location,
                                onChange: (e) => setNewRestaurant({...newRestaurant, location: e.target.value}),
                                className: 'px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent'
                            })
                        ),

                        React.createElement('div', { className: 'mb-6 p-4 bg-white rounded-lg border' },
                            React.createElement('div', { className: 'flex items-center' },
                                React.createElement(Package, { className: 'w-5 h-5 mr-2 text-gray-600' }),
                                React.createElement('label', { className: 'text-sm font-medium text-gray-700 mr-4' }, 'Is this takeout?'),
                                React.createElement('div', { className: 'flex items-center space-x-4' },
                                    React.createElement('label', { className: 'flex items-center' },
                                        React.createElement('input', {
                                            type: 'radio',
                                            name: 'takeout',
                                            checked: !newRestaurant.isTakeout,
                                            onChange: () => setNewRestaurant({...newRestaurant, isTakeout: false}),
                                            className: 'mr-2'
                                        }),
                                        React.createElement('span', { className: 'text-sm' }, 'No (Dine-in)')
                                    ),
                                    React.createElement('label', { className: 'flex items-center' },
                                        React.createElement('input', {
                                            type: 'radio',
                                            name: 'takeout',
                                            checked: newRestaurant.isTakeout,
                                            onChange: () => setNewRestaurant({...newRestaurant, isTakeout: true}),
                                            className: 'mr-2'
                                        }),
                                        React.createElement('span', { className: 'text-sm' }, 'Yes (Takeout)')
                                    )
                                )
                            )
                        ),

                        React.createElement('div', { className: 'grid md:grid-cols-2 gap-6' },
                            React.createElement('div', null,
                                React.createElement(ScoreSlider, {
                                    label: 'Was the Food Good?',
                                    value: newRestaurant.foodQuality,
                                    onChange: (value) => setNewRestaurant({...newRestaurant, foodQuality: value}),
                                    icon: Utensils
                                }),
                                newRestaurant.isTakeout ? 
                                    React.createElement(ScoreSlider, {
                                        label: 'Travels Well',
                                        value: newRestaurant.travelsWell,
                                        onChange: (value) => setNewRestaurant({...newRestaurant, travelsWell: value}),
                                        icon: Package
                                    }) :
                                    React.createElement(ScoreSlider, {
                                        label: 'Annoyance Level (10 = least annoying)',
                                        value: newRestaurant.annoyanceLevel,
                                        onChange: (value) => setNewRestaurant({...newRestaurant, annoyanceLevel: value}),
                                        icon: AlertTriangle
                                    })
                            ),
                            React.createElement('div', null,
                                React.createElement(ScoreSlider, {
                                    label: 'Taste to Price Scale',
                                    value: newRestaurant.tasteToPrice,
                                    onChange: (value) => setNewRestaurant({...newRestaurant, tasteToPrice: value}),
                                    icon: DollarSign
                                }),
                                React.createElement(ScoreSlider, {
                                    label: 'Do I Want to Go Back?',
                                    value: newRestaurant.wantToReturn,
                                    onChange: (value) => setNewRestaurant({...newRestaurant, wantToReturn: value}),
                                    icon: Heart
                                })
                            )
                        ),

                        React.createElement('div', { className: 'flex items-center justify-between mt-6' },
                            React.createElement('div', { className: 'text-lg font-semibold text-gray-700' },
                                'Total Score: ',
                                React.createElement('span', { className: 'text-blue-600' },
                                    `${newRestaurant.foodQuality + 
                                       (newRestaurant.isTakeout ? newRestaurant.travelsWell : newRestaurant.annoyanceLevel) + 
                                       newRestaurant.tasteToPrice + 
                                       newRestaurant.wantToReturn}/40`
                                )
                            ),
                            React.createElement('div', { className: 'flex gap-3' },
                                React.createElement('button', {
                                    onClick: () => setShowAddForm(false),
                                    className: 'px-4 py-2 text-gray-600 border border-gray-300 rounded-lg hover:bg-gray-50'
                                }, 'Cancel'),
                                React.createElement('button', {
                                    onClick: handleAddRestaurant,
                                    className: 'px-6 py-2 bg-green-600 text-white rounded-lg hover:bg-green-700 transition-colors'
                                }, 'Save Restaurant')
                            )
                        )
                    )
                ),

                restaurants.length === 0 ? 
                    React.createElement('div', { className: 'text-center py-12 bg-white rounded-lg shadow' },
                        React.createElement(Utensils, { className: 'w-16 h-16 mx-auto text-gray-400 mb-4' }),
                        React.createElement('h3', { className: 'text-xl font-semibold text-gray-600 mb-2' }, 'No restaurants yet!'),
                        React.createElement('p', { className: 'text-gray-500 mb-4' }, 'Start building your personal restaurant database by adding your first restaurant.'),
                        React.createElement('button', {
                            onClick: () => setShowAddForm(true),
                            className: 'px-6 py-3 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition-colors'
                        }, 'Add Your First Restaurant')
                    ) :
                    React.createElement('div', { className: 'space-y-4' },
                        restaurants.map((restaurant) =>
                            React.createElement('div', { key: restaurant.id, className: 'bg-white rounded-lg shadow p-6' },
                                React.createElement('div', { className: 'flex justify-between items-start mb-4' },
                                    React.createElement('div', null,
                                        React.createElement('div', { className: 'flex items-center mb-1' },
                                            React.createElement('h3', { className: 'text-xl font-bold text-gray-800' }, restaurant.name),
                                            restaurant.isTakeout && React.createElement('span', { 
                                                className: 'ml-2 px-2 py-1 bg-orange-100 text-orange-800 text-xs rounded-full' 
                                            }, 'Takeout')
                                        ),
                                        restaurant.location && React.createElement('p', { className: 'text-gray-600' }, restaurant.location),
                                        React.createElement('p', { className: 'text-sm text-gray-500' }, `Added: ${restaurant.dateAdded}`)
                                    ),
                                    React.createElement('div', { 
                                        className: `px-4 py-2 rounded-full font-bold text-lg ${getScoreColor(restaurant.totalScore)}` 
                                    }, `${restaurant.totalScore}/40`)
                                ),
                                
                                React.createElement('div', { className: 'grid grid-cols-2 md:grid-cols-4 gap-4' },
                                    React.createElement('div', { className: 'text-center p-3 bg-blue-50 rounded-lg' },
                                        React.createElement(Utensils, { className: 'w-6 h-6 mx-auto mb-1 text-blue-600' }),
                                        React.createElement('div', { className: 'font-semibold text-blue-800' }, `${restaurant.foodQuality}/10`),
                                        React.createElement('div', { className: 'text-xs text-gray-600' }, 'Food Quality')
                                    ),
                                    restaurant.isTakeout ? 
                                        React.createElement('div', { className: 'text-center p-3 bg-purple-50 rounded-lg' },
                                            React.createElement(Package, { className: 'w-6 h-6 mx-auto mb-1 text-purple-600' }),
                                            React.createElement('div', { className: 'font-semibold text-purple-800' }, `${restaurant.travelsWell}/10`),
                                            React.createElement('div', { className: 'text-xs text-gray-600' }, 'Travels Well')
                                        ) :
                                        React.createElement('div', { className: 'text-center p-3 bg-orange-50 rounded-lg' },
                                            React.createElement(AlertTriangle, { className: 'w-6 h-6 mx-auto mb-1 text-orange-600' }),
                                            React.createElement('div', { className: 'font-semibold text-orange-800' }, `${restaurant.annoyanceLevel}/10`),
                                            React.createElement('div', { className: 'text-xs text-gray-600' }, 'Low Annoyance')
                                        ),
                                    React.createElement('div', { className: 'text-center p-3 bg-green-50 rounded-lg' },
                                        React.createElement(DollarSign, { className: 'w-6 h-6 mx-auto mb-1 text-green-600' }),
                                        React.createElement('div', { className: 'font-semibold text-green-800' }, `${restaurant.tasteToPrice}/10`),
                                        React.createElement('div', { className: 'text-xs text-gray-600' }, 'Value')
                                    ),
                                    React.createElement('div', { className: 'text-center p-3 bg-red-50 rounded-lg' },
                                        React.createElement(Heart, { className: 'w-6 h-6 mx-auto mb-1 text-red-600' }),
                                        React.createElement('div', { className: 'font-semibold text-red-800' }, `${restaurant.wantToReturn}/10`),
                                        React.createElement('div', { className: 'text-xs text-gray-600' }, 'Return Desire')
                                    )
                                )
                            )
                        )
                    )
            );
        };

        ReactDOM.render(React.createElement(RestaurantRatingApp), document.getElementById('root'));
    </script>
</body>
</html>
Add Restaurant Directory App
